> 作者: sugar
> 
> 转载请联系作者(shigang-97@qq.com ),并说明出处!



## 1. 介绍
使用思源官方的docker镜像进行部署, 可以使用docker或者docker-compose. 使用docker相对来说比较简单, 但是使用docker-compose可以更方便的进行更新迭代. 具体选择哪个需要看个人选择.
下面会介绍分别使用两种方式进行云端部署以及软件更新.


## 2. 使用docker安装
### 2.1. 拉取siyuan docker镜像[^2]
```bash
docker pull b3log/siyuan
```
### 2.2. 启动服务

`siyuan`服务默认的workspace是`~/Documents/SiYuan`. 而默认启动容器时的用户是`siyuan`, workspace的路径就是`/home/siyuan/Documents/SiYuan`. 此使, 如果使用容器卷进行mount, 就会出现权限的问题, 出现文件无法写入的问题. 应该docker里的siyuan用户无法写入文件到主机的mount目录下. 
因此, 需要使用root用户来启动容器, 但是此时的workspace就会变为`/root/Documents/SiYuan`, 因此需要手动指定workspace的路径为`/home/siyuan/Documents/SiYuan`
此时, 创建的Document就属于root用户, 里面所有的数据也都属于root用户. 新添加的笔记, 图片,附件等也都属于root. 
![[Pasted image 20220120233133.png]]
![[Pasted image 20220120233254.png]]
![[Pasted image 20220120233353.png]]


以siyuan用户启动容器并进行mount的方法暂时没搞会, 需要进一步的了解docker的用户权限机制. 暂时能使用root用户完成功能, 能用即可. 数据的备份也OK.


```bash
docker run --name siyuan -it -d --restart=always -v /home/siyuan/Documents/SiYuan:/root/Documents/SiYuan -p 6806:6806 b3log/siyuan  # mount失败

docker run --name siyuan -it --restart=always -v /root/SiYuan:/home/siyuan/Documents/SiYuan -p 6806:6806 b3log/siyuan -resident -workspace /home/siyuan/Documents/SiYuan -accessAuthCode bfd6bedb # 没有权限

docker run --name siyuan -it --user=root  --restart=always -v /home/SiYuan:/home/siyuan/Documents/SiYuan -p 6806:6806 b3log/siyuan -workspace /home/siyuan/Documents/SiYuan
## --name siyuan 启用一个容器名为 siyuan 的容器 
## --restart=always 容器自启(正常写笔记的时候思源服务正常,但是我将一写markdown格式错误的笔记粘贴到笔记中之后,整理笔记的时候偶尔会崩溃) 
## -v /usr/local/siyuan/data/SiYuan:/root/Documents/SiYuan 将思源笔记的数据映射到服务器,在服务器的 /usr/local/siyuan/data/SiYuan 中就能看到所有的笔记了 
## -p 6806:6806 端口映射 
## 使用的容器 b3log/siyuan
## --user=root 使用root用户开始容器, 否则会提示权限问题, 无法mount
```

> 思源笔记目前必须使用6806端口, 其他端口会无法使用
启动之后, 需要检查防火墙设置, 6806号端口是否打开, 如果设置正常. 直接 `http://ip:6806`即可访问


### 2.3. 数据备份
```bash
# 进入docker查看数据
docker exec -it siyuan /bin/sh
```

```bash
docker stop siyuan
cd <siyuan_data_path>
sudo tar -zcvf SiYuan.tar.gz ./SiYuan  # 打包数据为压缩包
docker start siyuan
```

> 数据备份现在有问题. 不知道为什么主机的数据文件夹里面没有数据. [^2]

解决办法: 可能需要修改run的命令, 手动设置思源的 workspace 选项.


### 2.4. docker升级
```bash
docker pull b3log/siyuan ## 拉取新的docker
docker stop siyuan ## 停止容器
docker rm siyuan ## 删除容器
## 重新生成容器
docker run  --name siyuan  -it -d  --restart=always  -v /usr/local/software/siyuan/data/SiYuan:/root/Documents/SiYuan -p 6806:6806 b3log/siyuan
```


## 3. 使用docker-compose安装

```bash
curl -L https://get.daocloud.io/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose


version: '3'

services:
  siyuan:
    image: b3log/siyuan:latest
    container_name: siyuan
    restart: always
    volumes:
      - /root/SiYuan:/root/Documents/SiYuan
    command: [--resident=true,--workspace=/root/Documents/SiYuan,--ssl=true,--accessAuthCode=授权码,--servePath=绑定域名]
    network_mode: "host"
```
## 4. reference
1. [思源笔记 docker 搭建及后续使用优化（小白向）](https://ld246.com/article/1635558111871/comment/1635742197762)
2. [思源笔记docker部署](https://www.cnblogs.com/ziyue7575/p/15339826.html)
3. [思源笔记社区docker专栏](https://ld246.com/tag/docker)
4. [群晖docker部署、升级、外网访问 思源笔记](https://bright.htyed.top/index.php/archives/149/)



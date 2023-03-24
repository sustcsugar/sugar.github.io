> 作者: sugar
> 
> 转载请联系作者(shigang-97@qq.com ),并说明出处!


## 1. docker


### 1.1. 启动docker
docker是服务器-客户端架构, 因此, 如果需要使用`docker`命令, 需要本机启动`docker`服务:
```bash
systemctl start docker #systemctl用法
service docker start  #service用法
```

由于docker的启动需要sudo权限, 为了避免每次使用都输入sudo, 可以将用户加入docker用户组
```bash
sudo usermode -aG docker $USER
```


## 2. image
Docker 把应用程序及其依赖，打包在 image 文件里面。只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。

相关命令:[^3][^1]
```bash
#列出本机的所有image
docker imags
docker images -aq
# 参数
-a, --all # 列出所有镜像
-q, --quiet # 只显示镜像的ID

# 删除image
docker image rm [imageName]
docker rmi -f [imageID]

# 删除所有的images
docker rmi -f $(docker images -aq)
```

```shell
[root@sugarlearn ~]# docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
docker.io/b3log/siyuan    latest              a35a2a94fc5b        16 hours ago        142 MB
docker.io/zmister/mrdoc   v1                  1f0e4ae2440e        12 days ago         839 MB

#解释：
- REPOSITORY  仓库源
- TAG         镜像的标签
- IMAGE ID    镜像的ID
- CREATED     镜像创建的时间
- SIZE        镜像的大小


```

### 2.1. docler search 搜索镜像
```bash
docker search [imageName]
```

```shell
[root@sugarlearn ~]# docker search siyuan
INDEX       NAME                                           DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
docker.io   docker.io/b3log/siyuan                         思源笔记 Docker 镜像。SiYuan Docker Image.             27
docker.io   docker.io/siyuanhuang95/livox_slam             This docker offers you a convenient develo...   3
docker.io   docker.io/hhxsy/siyuan_xie                     myRepository                                    0
docker.io   docker.io/lsy643/lsy643                        Docker images from Siyuan Li                    0
docker.io   docker.io/nn200433/siyuan                      基于 b3log/siyuan ，删除默认siyuan用户                   0
docker.io   docker.io/siyuan0/learning                                                                     0
docker.io   docker.io/siyuan0/pytorch_chensy                                                               0
docker.io   docker.io/siyuan0322/hello                     hello world project of docker                   0
docker.io   docker.io/siyuan0322/mpi                                                                       0
docker.io   docker.io/siyuan0322/sshd                                                                      0
docker.io   docker.io/siyuan89/app01                                                                       0
docker.io   docker.io/siyuanchen726/docker101tutorial                                                      0
docker.io   docker.io/siyuangao/cni_challenge                                                              0
docker.io   docker.io/siyuangao/cni_challenge_dockerrepo                                                   0
docker.io   docker.io/siyuanli/cs503_tensorflow_jupyter                                                    0
docker.io   docker.io/siyuanli/onlinejudge                                                                 0
docker.io   docker.io/siyuanwlw/cheers2019                                                                 0
docker.io   docker.io/siyuanzhang0519/cheers2019                                                           0
docker.io   docker.io/siyuanzhu/friendlyhello                                                              0
docker.io   docker.io/siyuanzhu/toy-flask                                                                  0
docker.io   docker.io/taosiyuan/influxdb                   Created by siyuan.tao on 2017-3-22              0
docker.io   docker.io/xbdz/siyuan                          思源笔记镜像                                          0
```

### 2.2. docker pull 下载镜像
```bash
[root@sugarlearn ~]# docker pull jonnyan404/mrdoc
Using default tag: latest   # 默认下载最新版本，如果不加tag 就下载默认的最新版
Trying to pull repository docker.io/jonnyan404/mrdoc ...
latest: Pulling from docker.io/jonnyan404/mrdoc
59bf1c3509f3: Already exists   # 分层下载，已存在的层，就无需重新下载，联合文件系统思想
8786870f2876: Pull complete
45d4696938d0: Pull complete
ef84af58b2c5: Pull complete
c3c9b71b9a69: Pull complete
2f08af9381b7: Pull complete
7c154285520e: Pull complete
ab0cf852a137: Pull complete
Digest: sha256:dcb21f432b0ba6fb44284b0efe195a3701fda2a91c7c182b05d5fbfad632e328  # 签名
Status: Downloaded newer image for docker.io/jonnyan404/mrdoc:latest    # 真实地址
```

指定版本进行下载
```bash
docker pull jonnyan404/mrdoc:<version>
```

### 2.3. docker镜像原理

### 2.4. 生成自己的image
 [[#5 dockerfile]]

## 3. container
> 有了镜像才可以创建容器

### 3.1. docker 容器的存储位置
docker相关的所有数据都保存在主机的这个目录:`/var/lib/docker/`
创建的容器, 下载的镜像等等

### 3.2. docker run
```bash
docker run [可选参数] image

# 参数说明
-name="Name"   容器名字, image1 image2用来区分容器
-d             后台运行
-it            交互式运行容器
-p             指定容器端口
	-p 主机端口:容器端口(常用)
	-p 容器端口
-P             随机指定端口
```

### 3.3. docker ps
```bash
docker ps # 列出正在运行的容器
docker ps -q # 只列出ID

```

### 3.4. 退出容器
```bash
exit # 停止运行, 退出容器
ctrl + P + Q # 退出不停止
```

### 3.5. 删除容器
```bash
docker rm ID
docker rm -f $(docker ps -q)
docker ps -aq | xargs docker rm
```

### 3.6. 启动停止容器
```bash
docker start 容器ID # 启动容器
docker restat ID   #重启容器
docker stop ID     #停止容器
docker kill ID     #强制停止
```

### 3.7. 其他命令

```bash
[root@sugarlearn ~]# docker run -d centos
ca6b8a3a36e6b175e1bbbea8106a2365f5f410827a3e27687d61da510b2ae662
[root@sugarlearn ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

# 问题: 后台启动centos镜像, docker并没有启动

docker使用后台运行, 就必须有一个前台进程, docker发现没有应用,就会自动停止
```

```bash
# 查看日志
[root@sugarlearn ~]# docker logs --help

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --help           Print usage
      --since string   Show logs since timestamp
      --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps



# 写一个持续运行的脚本,放入后台运行, 使用log打印日志
[root@sugarlearn ~]# docker run -d centos /bin/bash -c "while true; do echo sugar; sleep 1; done"
2a780fc5f68cd62e0d0a99ece3bb4401283066587276e45de41de8c7498cb57f
[root@sugarlearn ~]# docker logs -ft --tail 10 2a780fc5f68cd62e0d0a99ece3bb4401283066587276e45de41de8c7498cb57f
2022-01-19T19:09:02.938158000Z sugar
2022-01-19T19:09:03.941165000Z sugar
2022-01-19T19:09:04.944188000Z sugar
2022-01-19T19:09:05.947294000Z sugar
2022-01-19T19:09:06.950443000Z sugar
2022-01-19T19:09:07.953860000Z sugar
2022-01-19T19:09:08.957836000Z sugar
2022-01-19T19:09:09.961211000Z sugar
2022-01-19T19:09:10.963906000Z sugar
2022-01-19T19:09:11.967134000Z sugar
2022-01-19T19:09:12.971447000Z sugar
2022-01-19T19:09:13.974090000Z sugar
```

```bash
# 查看容器的进程信息
docker top ID 
```


```bash
# 查看容器元数据
docker inspect

[root@sugarlearn ~]# docker inspect 2a780fc5f68cd62e0d0a99ece3bb4401283066587276e45de41de8c7498cb57f
[
    {
        "Id": "2a780fc5f68cd62e0d0a99ece3bb4401283066587276e45de41de8c7498cb57f",
        "Created": "2022-01-19T19:08:56.65117041Z",
        "Path": "/bin/bash",
        "Args": [
            "-c",
            "while true; do echo sugar; sleep 1; done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 3374,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-01-19T19:08:56.91832488Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/2a780fc5f68cd62e0d0a99ece3bb4401283066587276e45de41de8c7498cb57f/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/2a780fc5f68cd62e0d0a99ece3bb4401283066587276e45de41de8c7498cb57f/hostname",
        "HostsPath": "/var/lib/docker/containers/2a780fc5f68cd62e0d0a99ece3bb4401283066587276e45de41de8c7498cb57f/hosts",
        "LogPath": "",
        "Name": "/festive_euler",
        "RestartCount": 0,
        "Driver": "overlay2",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "journald",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "docker-runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": null,
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DiskQuota": 0,
            "KernelMemory": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": -1,
            "OomKillDisable": false,
            "PidsLimit": 0,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0
        },
        "GraphDriver": {
            "Name": "overlay2",
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/619adce5a0d2a1b8745d4f3935c9b1dc6c30c4494e46ce32e68cfaccb80b2853-init/diff:/var/lib/docker/overlay2/34e966a6c48160e1928a33a3bf332e2da80f374b51dbe603cf98e64d4b379fa2/diff",
                "MergedDir": "/var/lib/docker/overlay2/619adce5a0d2a1b8745d4f3935c9b1dc6c30c4494e46ce32e68cfaccb80b2853/merged",
                "UpperDir": "/var/lib/docker/overlay2/619adce5a0d2a1b8745d4f3935c9b1dc6c30c4494e46ce32e68cfaccb80b2853/diff",
                "WorkDir": "/var/lib/docker/overlay2/619adce5a0d2a1b8745d4f3935c9b1dc6c30c4494e46ce32e68cfaccb80b2853/work"
            }
        },
        "Mounts": [],
        "Config": {
            "Hostname": "2a780fc5f68c",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash",
                "-c",
                "while true; do echo sugar; sleep 1; done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "fc0b45deead7c901e02710a67c6290a197e19d77d8e32266211c617d899c329a",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/fc0b45deead7",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "da3db9f9ccbbbeb563c2aadfbb8938503cf9b2c82920893e9519a6365ccc3e51",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "b0fba45bae2a26e0e2c33eb5cf9050d48159091e9410d031cd83721db0fe436b",
                    "EndpointID": "da3db9f9ccbbbeb563c2aadfbb8938503cf9b2c82920893e9519a6365ccc3e51",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02"
                }
            }
        }
    }
]


```


```bash
# 进入正在运行的容器
# 通常都是使用后台运行的容器, 需要进入后台运行的容器, 修改一些配置


# method 1: 进入容器, 开启新的终端, 可以在里面操作
docker exec -it containerID 


# method 2: 进入正在执行的终端,不会启动新的进程
docker attach containerID

[root@sugarlearn ~]# docker attach 2a780fc5f68c
sugar
sugar
sugar
sugar
sugar
sugar
sugar
sugar
```

```bash
# 从容器拷贝文件出来
docker cp containerID:path host_path

```


```bash
# 启动并进入容器,主机名就是ID名
[root@sugarlearn ~]# docker run -it centos /bin/bash
[root@8ec75bda8fcd /]#
[root@8ec75bda8fcd /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```



## 4. 实例
### 4.1. docker安装nginx
```bash
[root@sugarlearn ~]# docker run -d --name nginx01 -p 3344:80 nginx  # -p 主机端口:docker端口, 将docker的端口映射到主机. nginx的默认端口为80, 映射之后, 在主机的3344端口可以访问到容器的80端口
e87155516f5b0c2ea566543eab6f54108ceb997b0dd2e56b78605bf18e09c465
[root@sugarlearn ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
e87155516f5b        nginx               "/docker-entrypoin..."   19 seconds ago      Up 19 seconds       0.0.0.0:3344->80/tcp   nginx01
[root@sugarlearn ~]# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
[root@sugarlearn ~]#

```
### 4.2. docker安装tomcat
```bash
docker run -it --rm tomcat:9.0

# --rm表示用完即删, 停止容器之后,立即就删除容器. 


# 正常启动tomcat, 映射端口为3344
docker run -d -p 3344:8080 --name tomcat01 tomcat
# 进入容器
docker exrc -it tomcat01 /bin/bash
# 将webapp.dist里面的内容拷贝到webapps里面, 打开ip:3344即可访问成功, 如下图所示
```
![[Pasted image 20220120193434.png]]

### 4.3. docker部署es+kibana
> es暴露的端口很多
> es十分的耗内存
> es的数据一般需要挂载出去, 放置到安全目录

```bash
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:tag
# --net 网络设置

# 去掉网络环境设置, 使用7.6.2版本
docker run -d --name elasticsearch  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2
# 启动服务之后, 系统负载十分大,linux直接卡住了

# 使用 docker stats 查看cpu的状态
docker status

# 测试es是否成功

# 关闭es, 增加内存的限制
docker run -d --name elasticsearch  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -eES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
```

## 5. 容器数据卷
 数据持久化, 不保存在容器中 
 卷技术: 目录的挂载, 将容器内的目录挂载到linux上面. 可以将容器的数据和linux对应路径进行同步.

>  容器之间也可以进行数据共享

 挂载是将主机的路径挂载到容器内, 因此主机是 source. 容器是 destination

### 5.1. 容器数据卷挂载到主机
#### 5.1.1. 方式1. 启动容器时, 使用命令来挂载 -v
 
```bash
docker run -it -v 主机目录:容器内目录
```

```bash
# 使用docker inspect 查看"Mounts": [
	{
		"Type": "bind",
		"Source": "/home/ceshi", # 主机的地址
		"Destination": "/home",  # 容器的地址
		"Mode": "",
		"RW": true,
		"Propagation": "rprivate"
	}
],

```

没有权限处理办法: `docker run -it --privileged=true -v /home/ceshi:/home centos /bin/bash`. (该方法好像失效了, 可以在启动容器的时候, 指定以root用户启动, 就会有权限了. )

#### 5.1.2. 方式2. 通过dockerfile进行挂载
dockerfile 就是用来构建docker镜像的构建文件. 是命令脚本

```bash
# 创建dockerfile文件
# 文件内容如下
# 指定(大写) 参数
# 每个命令就是image的一层

FROM centos

VOLUME ["volume01","volume02"]

CMD echo "---end---"
CMD /bin/bash
```

运行该脚本, 生成一个新的镜像:
通过`build`进行镜像的构建, `-t`表示`target`, 生成的镜像名和版本号为`shigang/centos:1.0`, 生成的路径`.`表示当前路径下
```bash
docker build -f dockerfile -t shigang/cantos:1.0 .
```

运行生成的image, 查看是否挂载. 可以发现在根目录下已经有了volume01和volume02两个卷. 脚本中写的是匿名挂载方式. 
```bash
[root@sugarlearn docker-test-volume]# docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
82a6ceebcf8a        b3log/siyuan        "/opt/siyuan/kerne..."   4 hours ago         Up 4 hours          0.0.0.0:6806->6806/tcp     siyuan
04b9ad86bdb6        zmister/mrdoc:v1    "sh docker_mrdoc.sh"     24 hours ago        Up 24 hours         0.0.0.0:10086->10086/tcp   mrdoc
[root@sugarlearn docker-test-volume]# docker run -it 7b064e07c425 /bin/bash
[root@c1e73c7792a5 /]# ls -l
total 56
lrwxrwxrwx   1 root root    7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x   5 root root  360 Jan 20 19:53 dev
drwxr-xr-x   1 root root 4096 Jan 20 19:53 etc
drwxr-xr-x   2 root root 4096 Nov  3  2020 home
lrwxrwxrwx   1 root root    7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Nov  3  2020 lib64 -> usr/lib64
drwx------   2 root root 4096 Sep 15 14:17 lost+found
drwxr-xr-x   2 root root 4096 Nov  3  2020 media
drwxr-xr-x   2 root root 4096 Nov  3  2020 mnt
drwxr-xr-x   2 root root 4096 Nov  3  2020 opt
dr-xr-xr-x 141 root root    0 Jan 20 19:53 proc
dr-xr-x---   2 root root 4096 Sep 15 14:17 root
drwxr-xr-x   1 root root 4096 Jan 20 19:53 run
lrwxrwxrwx   1 root root    8 Nov  3  2020 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Nov  3  2020 srv
dr-xr-xr-x  13 root root    0 Jan 19 07:39 sys
drwxrwxrwt   7 root root 4096 Sep 15 14:17 tmp
drwxr-xr-x  12 root root 4096 Sep 15 14:17 usr
drwxr-xr-x  20 root root 4096 Sep 15 14:17 var
drwxr-xr-x   2 root root 4096 Jan 20 19:53 volume01
drwxr-xr-x   2 root root 4096 Jan 20 19:53 volume02
```

通过`docker inspect ID` 查看容器信息, 可以发现信息已经同步. 可以在容器中volume01创建文件夹,, 之后再宿主机查看是否有对应的文件. 
```bash
"Mounts": [
	{
		"Type": "volume",
		"Name": "a6255c3d8111e65ef8dc62e2d8f0b951899f0c6d36455276daf384c6fc41861a",
		"Source": "/var/lib/docker/volumes/a6255c3d8111e65ef8dc62e2d8f0b951899f0c6d36455276daf384c6fc41861a/_data",
		"Destination": "volume01",
		"Driver": "local",
		"Mode": "",
		"RW": true,
		"Propagation": ""
	},
	{
		"Type": "volume",
		"Name": "0ec9fb36ce7235343426da000088f63115d99e603206ee0f9af5dd1bac6b3f4a",
		"Source": "/var/lib/docker/volumes/0ec9fb36ce7235343426da000088f63115d99e603206ee0f9af5dd1bac6b3f4a/_data",
		"Destination": "volume02",
		"Driver": "local",
		"Mode": "",
		"RW": true,
		"Propagation": ""
	}
],

```


### 5.2. 多容器互相挂载
> 多个容器之间同步数据, 使用 `--volimes-from` 表示从另一个docker容器中获取挂载目录

通过启动三个容器测试学习

在[[#方式2 通过dockerfile进行挂载]]的时候, 使用dockerfile创建了自己的image, 使用我们自己创建的image为例, 进行多个容器的容器卷挂载测试

1. 启动第一个容器
	```bash
	# 需要注意的是, centos系统容器直接使用exit退出后, 由于没有前台应用, 会自动停止运行
	# 因此需要使用 ctrl + P + Q 退出容器
	[root@sugarlearn home]# docker run -it --name docker01 7b064e07c425
	[root@815381166fd9 /]# ls
	bin  etc   lib    lost+found  mnt  proc  run   srv  tmp  var       volume02
	dev  home  lib64  media       opt  root  sbin  sys  usr  volume01
	```

1. 启动第二个容器, 发现和第一个容器的挂载目录一样
	```bash
	[root@sugarlearn home]# docker run -it --name docker02 --volumes-from docker01 7b064e07c425
	[root@9b5b20dec559 /]# ls
	bin  etc   lib    lost+found  mnt  proc  run   srv  tmp  var       volume02
	dev  home  lib64  media       opt  root  sbin  sys  usr  volume01
	```

2. 在`docker01`中的`volume01`中添加文件, 查看`docker02`中是否出现. 实验结果发现, 两个容器的卷是互通的
	```bash
	[root@9b5b20dec559 /]# cd volume01
	[root@9b5b20dec559 volume01]# ls
	[root@9b5b20dec559 volume01]# touch 123
	[root@9b5b20dec559 volume01]# touch 12345
	[root@9b5b20dec559 volume01]# ls
	123  12345
	[root@9b5b20dec559 volume01]# [root@sugarlearn home]#
	[root@sugarlearn home]#
	[root@sugarlearn home]# docker ps
	CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
	9b5b20dec559        7b064e07c425        "/bin/sh -c /bin/bash"   4 minutes ago       Up 4 minutes                                   docker02
	815381166fd9        7b064e07c425        "/bin/sh -c /bin/bash"   8 minutes ago       Up 8 minutes                                   docker01
	82a6ceebcf8a        b3log/siyuan        "/opt/siyuan/kerne..."   5 hours ago         Up 4 hours          0.0.0.0:6806->6806/tcp     siyuan
	04b9ad86bdb6        zmister/mrdoc:v1    "sh docker_mrdoc.sh"     24 hours ago        Up 24 hours         0.0.0.0:10086->10086/tcp   mrdoc
	[root@sugarlearn home]# docker attach 815381166fd9
	[root@815381166fd9 /]# ls
	bin  etc   lib    lost+found  mnt  proc  run   srv  tmp  var       volume02
	dev  home  lib64  media       opt  root  sbin  sys  usr  volume01
	[root@815381166fd9 /]# cd volume01/
	[root@815381166fd9 volume01]# ls
	123  12345

	```

1. 创建docker03, 发现也可以挂载
	```bash
	[root@sugarlearn home]# docker run -it --name docker03 --volumes-from docker01 shigang/centos:1.0
	[root@8fbd45b8dd55 /]# ls
	bin  etc   lib    lost+found  mnt  proc  run   srv  tmp  var       volume02
	dev  home  lib64  media       opt  root  sbin  sys  usr  volume01
	[root@8fbd45b8dd55 /]# cd volume0
	bash: cd: volume0: No such file or directory
	[root@8fbd45b8dd55 /]# cd volume01
	[root@8fbd45b8dd55 volume01]# ls
	12038120938  123  12345  aksdjhaskdjh
	```

4. 将dicker01删除之后, 发现数据依然是存在的, docker02和03的数据卷挂载依然存在

### 5.3. 具名挂载和匿名挂载

> 一般都使用具名挂载, 此概念了解即可

匿名挂载:
```bash
-v 容器内路径
```


## 6. dockerfile
### 6.1. 构建步骤:
1. 编写dockerfile文件
2. `docker build` 构建成为一个镜像
3. `docker run` 运行镜像
4. `docker push` 发布镜像

> Tip: 官方名字是Dockerfile, 使用官方名字, 在build的时候就不需要使用`-f`指定文件了. 

### 6.2. 查看官方例子
以centos为例: [官方镜像地址](https://hub.docker.com/_/centos)
点击具体的版本号进去, 发现是github页面, 里面是dockerfile文件
很多官方的镜像都是基础包, 很多的功能是没有的, 我们通常会搭建自己的镜像
![[Pasted image 20220121043052.png]]
![[Pasted image 20220121043123.png]]
```bash
FROM scratch

ADD centos-7-x86_64-docker.tar.xz /

LABEL \

org.label-schema.schema-version="1.0" \

org.label-schema.name="CentOS Base Image" \

org.label-schema.vendor="CentOS" \

org.label-schema.license="GPLv2" \

org.label-schema.build-date="20201113" \

org.opencontainers.image.title="CentOS Base Image" \

org.opencontainers.image.vendor="CentOS" \

org.opencontainers.image.licenses="GPL-2.0-only" \

org.opencontainers.image.created="2020-11-13 00:00:00+00:00"

CMD ["/bin/bash"]
```

### 6.3. dockerfile组成
基础知识:
1. 每个关键字(指令)都必须是大写
2. 从上到下的执行顺序
3. \# 表示注释
4. 每个指令都会创建提交一个新的***镜像层***, 并提交.

![](https://img2020.cnblogs.com/blog/1031302/202009/1031302-20200915150122670-1388543274.png)

### 6.4. dockerfile的指令
指令汇总:
![](https://img2018.cnblogs.com/i-beta/631711/201912/631711-20191220153130957-348455400.png)

```
FROM    # 基础镜像, 一切从这里构建
MAINTAINER # 镜像的作者, 姓名+邮箱
RUN        # 镜像构建的时候
ADD        # 步骤
WORKDIR    # 镜像的工作目录
VOLUME     # 挂载的目录
EXPOSE     # 暴露端口

CMD        # 指定容器启动的时候, 运行的命令, 只有最后一个会生效, 且可被替代
ENTRYPOINT # 指定容器启动的时候, 运行的命令, 可以追加命令

ONBUILD    # 当构建一个被继承的dockerfile的时候, 就会运行 onbuild 的命令. 
COPY       # 类似于add, 将文件拷贝到镜像中

ENV        # 构建的时候设置环境变量
```
### 6.5. 使用dockerfile构建自己的centos系统
```bash
FROM scratch    # dockerhub中绝大多数的镜像, 都是从此基础镜像而来的
```

> 创建自己的centos

1. 编写配置文件
```bash
FROM centos
MAINTAINER sugar<shigang-97@qq.com >

ENV MYPATH /usr/local     
WORKDIR $MYPATH            # 设置工作目录, 即为进入容器内的目录. 官方的centos启动之后, 进入容器内部为根目录, 通过此设置, 可以将进入容器的默认目录进行修改

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash

```

2. 通过文件构建镜像
```bash
docker build -f dockfile -t mycentos:0.1 .
```
通过上面的命令, 使用 dockerfile构建构建新的镜像
```bash
[root@sugarlearn docker-test-volume]# docker build -f dockerfile -t mycentos:0.1 .
Sending build context to Docker daemon 2.048 kB
Step 1/10 : FROM centos
 ---> 5d0da3dc9764
Step 2/10 : MAINTAINER sugar<shigang-97@qq.com >
 ---> Running in b6fcc94bb8a5
 ---> 74c30c1cbe48
Removing intermediate container b6fcc94bb8a5
Step 3/10 : ENV MYPATH /usr/local
 ---> Running in 9f870d79569c
 ---> 2c28d28d5fd8
Removing intermediate container 9f870d79569c
Step 4/10 : WORKDIR $MYPATH
 ---> dfb3f984beb8
Removing intermediate container dd41b1c38380
Step 5/10 : RUN yum -y install vim
 ---> Running in b6c7b75645b7

CentOS Linux 8 - AppStream                      4.6 MB/s | 8.4 MB     00:01
CentOS Linux 8 - BaseOS                         4.0 MB/s | 4.6 MB     00:01
CentOS Linux 8 - Extras                          17 kB/s |  10 kB     00:00
Dependencies resolved.
================================================================================
 Package             Arch        Version                   Repository      Size
================================================================================
Installing:
 vim-enhanced        x86_64      2:8.0.1763-16.el8         appstream      1.4 M
Installing dependencies:
 gpm-libs            x86_64      1.20.7-17.el8             appstream       39 k
 vim-common          x86_64      2:8.0.1763-16.el8         appstream      6.3 M
 vim-filesystem      noarch      2:8.0.1763-16.el8         appstream       49 k
 which               x86_64      2.21-16.el8               baseos          49 k

Transaction Summary
================================================================================
Install  5 Packages

Total download size: 7.8 M
Installed size: 30 M
Downloading Packages:
(1/5): gpm-libs-1.20.7-17.el8.x86_64.rpm        221 kB/s |  39 kB     00:00
(2/5): vim-enhanced-8.0.1763-16.el8.x86_64.rpm  4.5 MB/s | 1.4 MB     00:00
(3/5): vim-filesystem-8.0.1763-16.el8.noarch.rp 172 kB/s |  49 kB     00:00
(4/5): which-2.21-16.el8.x86_64.rpm             309 kB/s |  49 kB     00:00
(5/5): vim-common-8.0.1763-16.el8.x86_64.rpm     12 MB/s | 6.3 MB     00:00
--------------------------------------------------------------------------------
Total                                           5.7 MB/s | 7.8 MB     00:01
warning: /var/cache/dnf/appstream-02e86d1c976ab532/packages/gpm-libs-1.20.7-17.el8.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID 8483c65d: NOKEY
CentOS Linux 8 - AppStream                      1.6 MB/s | 1.6 kB     00:00
Importing GPG key 0x8483C65D:
 Userid     : "CentOS (CentOS Official Signing Key) <security@centos.org>"
 Fingerprint: 99DB 70FA E1D7 CE22 7FB6 4882 05B5 55B3 8483 C65D
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1
  Installing       : which-2.21-16.el8.x86_64                               1/5
  Installing       : vim-filesystem-2:8.0.1763-16.el8.noarch                2/5
  Installing       : vim-common-2:8.0.1763-16.el8.x86_64                    3/5
  Installing       : gpm-libs-1.20.7-17.el8.x86_64                          4/5
  Running scriptlet: gpm-libs-1.20.7-17.el8.x86_64                          4/5
  Installing       : vim-enhanced-2:8.0.1763-16.el8.x86_64                  5/5
  Running scriptlet: vim-enhanced-2:8.0.1763-16.el8.x86_64                  5/5
  Running scriptlet: vim-common-2:8.0.1763-16.el8.x86_64                    5/5
  Verifying        : gpm-libs-1.20.7-17.el8.x86_64                          1/5
  Verifying        : vim-common-2:8.0.1763-16.el8.x86_64                    2/5
  Verifying        : vim-enhanced-2:8.0.1763-16.el8.x86_64                  3/5
  Verifying        : vim-filesystem-2:8.0.1763-16.el8.noarch                4/5
  Verifying        : which-2.21-16.el8.x86_64                               5/5

Installed:
  gpm-libs-1.20.7-17.el8.x86_64         vim-common-2:8.0.1763-16.el8.x86_64
  vim-enhanced-2:8.0.1763-16.el8.x86_64 vim-filesystem-2:8.0.1763-16.el8.noarch
  which-2.21-16.el8.x86_64

Complete!
 ---> 4c7b4ce9b341
Removing intermediate container b6c7b75645b7
Step 6/10 : RUN yum -y install net-tools
 ---> Running in 44cf17d41b1d

Last metadata expiration check: 0:00:10 ago on Mon Jan 24 09:43:16 2022.
Dependencies resolved.
================================================================================
 Package         Architecture Version                        Repository    Size
================================================================================
Installing:
 net-tools       x86_64       2.0-0.52.20160912git.el8       baseos       322 k

Transaction Summary
================================================================================
Install  1 Package

Total download size: 322 k
Installed size: 942 k
Downloading Packages:
net-tools-2.0-0.52.20160912git.el8.x86_64.rpm   1.2 MB/s | 322 kB     00:00
--------------------------------------------------------------------------------
Total                                           337 kB/s | 322 kB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1
  Installing       : net-tools-2.0-0.52.20160912git.el8.x86_64              1/1
  Running scriptlet: net-tools-2.0-0.52.20160912git.el8.x86_64              1/1
  Verifying        : net-tools-2.0-0.52.20160912git.el8.x86_64              1/1

Installed:
  net-tools-2.0-0.52.20160912git.el8.x86_64

Complete!
 ---> e2a85f441e55
Removing intermediate container 44cf17d41b1d
Step 7/10 : EXPOSE 80
 ---> Running in 96cbdbc3347b
 ---> 135f028da5a8
Removing intermediate container 96cbdbc3347b
Step 8/10 : CMD echo $MYPATH
 ---> Running in 4d092e681f27
 ---> d7877648b301
Removing intermediate container 4d092e681f27
Step 9/10 : CMD echo "---end---"
 ---> Running in 551d9ae06abe
 ---> c465707419b5
Removing intermediate container 551d9ae06abe
Step 10/10 : CMD /bin/bash
 ---> Running in 3ad32c2be5fd
 ---> 6996698dfbc1
Removing intermediate container 3ad32c2be5fd
Successfully built 6996698dfbc1
```
. 构建镜像之后, 可以进入容器查看命令是否安装
```bash
[root@sugarlearn docker-test-volume]# docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED              SIZE
mycentos                      0.1                 6996698dfbc1        About a minute ago   326 MB
[root@sugarlearn docker-test-volume]# docker run -it mycentos:0.1
[root@b22fae9e9ab5 local]# pwd
/usr/local
[root@b22fae9e9ab5 local]# vim     # 可以发现vim已经安装了
```

### 6.6. 列出镜像历史
```bash
[root@sugarlearn docker-test-volume]# docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mycentos                      0.1                 6996698dfbc1        12 minutes ago      326 MB
[root@sugarlearn docker-test-volume]# docker history 6996698dfbc1
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
6996698dfbc1        12 minutes ago      /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/b...   0 B
c465707419b5        12 minutes ago      /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "ec...   0 B
d7877648b301        12 minutes ago      /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "ec...   0 B
135f028da5a8        12 minutes ago      /bin/sh -c #(nop)  EXPOSE 80/tcp                0 B
e2a85f441e55        12 minutes ago      /bin/sh -c yum -y install net-tools             28.4 MB
4c7b4ce9b341        12 minutes ago      /bin/sh -c yum -y install vim                   66.3 MB
dfb3f984beb8        13 minutes ago      /bin/sh -c #(nop) WORKDIR /usr/local            0 B
2c28d28d5fd8        13 minutes ago      /bin/sh -c #(nop)  ENV MYPATH=/usr/local        0 B
74c30c1cbe48        13 minutes ago      /bin/sh -c #(nop)  MAINTAINER sugar<114439...   0 B
5d0da3dc9764        4 months ago        /bin/sh -c #(nop)  CMD ["/bin/bash"]            0 B
<missing>           4 months ago        /bin/sh -c #(nop)  LABEL org.label-schema....   0 B
<missing>           4 months ago        /bin/sh -c #(nop) ADD file:805cb5e15fb6e0b...   231 MB
[root@sugarlearn docker-test-volume]#

```

> 可以查看其他镜像的构建过程
```bash
[root@sugarlearn docker-test-volume]# docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mycentos                      0.1                 6996698dfbc1        14 minutes ago      326 MB
docker.io/b3log/siyuan        latest              68ac87c0ad39        3 days ago          142 MB
docker.io/jonnyan404/mrdoc    latest              a187113ddd2a        7 days ago          864 MB
docker.io/zmister/mrdoc       v1                  1f0e4ae2440e        2 weeks ago         839 MB
docker.io/nginx               latest              605c77e624dd        3 weeks ago         141 MB
docker.io/tomcat              latest              fb5657adc892        4 weeks ago         680 MB
docker.io/centos              latest              5d0da3dc9764        4 months ago        231 MB
docker.io/jellyfin/jellyfin   latest              0aa773b67433        4 months ago        717 MB
docker.io/elasticsearch       7.6.2               f29a1ee41030        22 months ago       791 MB
[root@sugarlearn docker-test-volume]# docker history 605c77e624dd
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
605c77e624dd        3 weeks ago         /bin/sh -c #(nop)  CMD ["nginx" "-g" "daem...   0 B
<missing>           3 weeks ago         /bin/sh -c #(nop)  STOPSIGNAL SIGQUIT           0 B
<missing>           3 weeks ago         /bin/sh -c #(nop)  EXPOSE 80                    0 B
<missing>           3 weeks ago         /bin/sh -c #(nop)  ENTRYPOINT ["/docker-en...   0 B
<missing>           3 weeks ago         /bin/sh -c #(nop) COPY file:09a214a3e07c91...   4.61 kB
<missing>           3 weeks ago         /bin/sh -c #(nop) COPY file:0fd5fca330dcd6...   1.04 kB
<missing>           3 weeks ago         /bin/sh -c #(nop) COPY file:0b866ff3fc1ef5...   1.96 kB
<missing>           3 weeks ago         /bin/sh -c #(nop) COPY file:65504f71f5855c...   1.2 kB
<missing>           3 weeks ago         /bin/sh -c set -x     && addgroup --system...   61.1 MB
<missing>           3 weeks ago         /bin/sh -c #(nop)  ENV PKG_RELEASE=1~bullseye   0 B
<missing>           3 weeks ago         /bin/sh -c #(nop)  ENV NJS_VERSION=0.7.1        0 B
<missing>           3 weeks ago         /bin/sh -c #(nop)  ENV NGINX_VERSION=1.21.5     0 B
<missing>           4 weeks ago         /bin/sh -c #(nop)  LABEL maintainer=NGINX ...   0 B
<missing>           4 weeks ago         /bin/sh -c #(nop)  CMD ["bash"]                 0 B
<missing>           4 weeks ago         /bin/sh -c #(nop) ADD file:09675d11695f65c...   80.4 MB
[root@sugarlearn docker-test-volume]#

```

### 6.7. CMD和ENTRYPOINT的区别
dockerfile中一下两个命令的区别
```bash
CMD        # 指定容器启动的时候, 运行的命令, 只有最后一个会生效, 且可被替代
ENTRYPOINT # 指定容器启动的时候, 运行的命令, 可以追加命令
```

**测试cmd**:
可以看到, 启动镜像之后, 直接执行了CMD中的命令, 工作目录的文件列了出来

构建dockerfile用来测试
```dockerfile
FROM centos
CMD ["ls","-a"]
```
构建并启动镜像
```bash
[root@sugarlearn docker-test-volume]# docker build -f dockerfile_cmd -t cmdtest:1.0 .
Sending build context to Docker daemon 3.072 kB
Step 1/2 : FROM centos
 ---> 5d0da3dc9764
Step 2/2 : CMD ls -a
 ---> Running in 436ea9052803
 ---> 9da23cd6ea1d
Removing intermediate container 436ea9052803
Successfully built 9da23cd6ea1d
[root@sugarlearn docker-test-volume]# docker images
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
cmdtest                       1.0                 9da23cd6ea1d        5 seconds ago       231 MB
mycentos                      0.1                 6996698dfbc1        17 minutes ago      326 MB
docker.io/b3log/siyuan        latest              68ac87c0ad39        3 days ago          142 MB
docker.io/jonnyan404/mrdoc    latest              a187113ddd2a        7 days ago          864 MB
docker.io/zmister/mrdoc       v1                  1f0e4ae2440e        2 weeks ago         839 MB
docker.io/nginx               latest              605c77e624dd        3 weeks ago         141 MB
docker.io/tomcat              latest              fb5657adc892        4 weeks ago         680 MB
docker.io/centos              latest              5d0da3dc9764        4 months ago        231 MB
docker.io/jellyfin/jellyfin   latest              0aa773b67433        4 months ago        717 MB
docker.io/elasticsearch       7.6.2               f29a1ee41030        22 months ago       791 MB
[root@sugarlearn docker-test-volume]# docker run 9da23cd6ea1d
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
[root@sugarlearn docker-test-volume]#
```

dockerfile中执行的命令为`ls -a`, 如果想追加命令为`ls -al`, 测试一下, 发现会报错.
使用CMD的情况下, -l 会替换 CMD中的命令["ls","-a"], 将`-l`认定为一个命令, 但是找不到此命令, 所以会报错.
如果想要`ls -al` 则必须输入完整的命令. 
```bash
[root@sugarlearn docker-test-volume]# docker run 9da23cd6ea1d -l
container_linux.go:235: starting container process caused "exec: \"-l\": executable file not found in $PATH"
/usr/bin/docker-current: Error response from daemon: oci runtime error: container_linux.go:235: starting container process caused "exec: \"-l\": executable file not found in $PATH".
[root@sugarlearn docker-test-volume]# docker run 9da23cd6ea1d ls -al
total 56
drwxr-xr-x   1 root root 4096 Jan 24 10:12 .
drwxr-xr-x   1 root root 4096 Jan 24 10:12 ..
-rwxr-xr-x   1 root root    0 Jan 24 10:12 .dockerenv
lrwxrwxrwx   1 root root    7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x   5 root root  340 Jan 24 10:12 dev
drwxr-xr-x   1 root root 4096 Jan 24 10:12 etc
drwxr-xr-x   2 root root 4096 Nov  3  2020 home
lrwxrwxrwx   1 root root    7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Nov  3  2020 lib64 -> usr/lib64
drwx------   2 root root 4096 Sep 15 14:17 lost+found
drwxr-xr-x   2 root root 4096 Nov  3  2020 media
drwxr-xr-x   2 root root 4096 Nov  3  2020 mnt
drwxr-xr-x   2 root root 4096 Nov  3  2020 opt
dr-xr-xr-x 148 root root    0 Jan 24 10:12 proc
dr-xr-x---   2 root root 4096 Sep 15 14:17 root
drwxr-xr-x   1 root root 4096 Jan 24 10:12 run
lrwxrwxrwx   1 root root    8 Nov  3  2020 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Nov  3  2020 srv
dr-xr-xr-x  13 root root    0 Jan 19 07:39 sys
drwxrwxrwt   7 root root 4096 Sep 15 14:17 tmp
drwxr-xr-x  12 root root 4096 Sep 15 14:17 usr
drwxr-xr-x  20 root root 4096 Sep 15 14:17 var
```


**测试ENTRYPOINT**
上述使用CMD追加命令的时候, 会将命令替换掉, 但是使用ENTRYPOINT则可以追加命令. 
测试一下. 
首先编写dockerfile
```dockerfile
FROM centos
CMD ["ls","-a"]
```
构建并执行镜像, 可以发现, 追加 `-l`可以正确的执行命令, 不会覆盖原来的命令
```bash
[root@sugarlearn docker-test-volume]# docker run entrypoint
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
[root@sugarlearn docker-test-volume]# docker run entrypoint -l
total 56
drwxr-xr-x   1 root root 4096 Jan 24 10:15 .
drwxr-xr-x   1 root root 4096 Jan 24 10:15 ..
-rwxr-xr-x   1 root root    0 Jan 24 10:15 .dockerenv
lrwxrwxrwx   1 root root    7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x   5 root root  340 Jan 24 10:15 dev
drwxr-xr-x   1 root root 4096 Jan 24 10:15 etc
drwxr-xr-x   2 root root 4096 Nov  3  2020 home
lrwxrwxrwx   1 root root    7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Nov  3  2020 lib64 -> usr/lib64
drwx------   2 root root 4096 Sep 15 14:17 lost+found
drwxr-xr-x   2 root root 4096 Nov  3  2020 media
drwxr-xr-x   2 root root 4096 Nov  3  2020 mnt
drwxr-xr-x   2 root root 4096 Nov  3  2020 opt
dr-xr-xr-x 147 root root    0 Jan 24 10:15 proc
dr-xr-x---   2 root root 4096 Sep 15 14:17 root
drwxr-xr-x   1 root root 4096 Jan 24 10:15 run
lrwxrwxrwx   1 root root    8 Nov  3  2020 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Nov  3  2020 srv
dr-xr-xr-x  13 root root    0 Jan 19 07:39 sys
drwxrwxrwt   7 root root 4096 Sep 15 14:17 tmp
drwxr-xr-x  12 root root 4096 Sep 15 14:17 usr
drwxr-xr-x  20 root root 4096 Sep 15 14:17 var
```

## 7. reference
1. [docker使用指南](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)
2. [docker建站](https://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)
3. [求求你了，用Docker吧](https://juejin.cn/post/7021006271818137630?share_token=878fa440-84d9-4d8a-980e-aaba73440daf)
4. [狂神说docker视频](https://www.bilibili.com/video/BV1og4y1q7M4)
5. [官方帮助文档](https://docs.docker.com/get-started/)
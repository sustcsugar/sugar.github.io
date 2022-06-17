> 作者: sugar
> 
> 转载请联系作者(1144392179@qq.com),并说明出处!


## 1. Introduction


docsify是基于nodejs的, 需要使用npm工具进行安装.

## 2. 安装

nodejs以及npm的安装使用, 在安装node以及npm的前提下, 安装 docfisy. 
```bash
npm install -g docsify-cli

# 安装完成之后 查看版本
[root@sugarlearn /]# docsify -v

docsify-cli version:
  4.4.3
```

## 3. 使用
docsify的使用很简单, 新建一个文件夹, 使用docsify将其初始化即可.  类似于git的初始化. 
初始化之后, 会在仓库中新建一个README.md以及index.html, 这两个就是我们的网页内容. 
docsify会将这个README.md文件渲染成一个网页, index.html就是网站的入口文件. 

```bash
# docsify初始化
[root@sugarlearn docsify_repo]# docsify init
. already exists.
✔ Are you sure you want to rewrite it? (y/N) true

Initialization succeeded! Please run docsify serve

[root@sugarlearn docsify_repo]#
[root@sugarlearn docsify_repo]# ls
index.html  README.md
```

初始化之后, 启动网站:
```bash
docsify serve
```

## 4. 配置
网页所有相关的配置, 都可以通过修改 index.html 文件来实现. 

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="description" content="Description">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0">
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify@4/lib/themes/vue.css">
</head>
<body>
  <div id="app"></div>
  <script>
    window.$docsify = {
      name: '',
      repo: ''
	  coverpage: true
	  loadNavbar: true
    }
  </script>
  <!-- Docsify v4 -->
  <script src="//cdn.jsdelivr.net/npm/docsify@4"></script>
</body>
</html>

```


### 4.1. 主题
目前支持以下几个主题,修改`vue.vss`为其他主题名即可. 
```html
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/vue.css"> 
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/buble.css"> 
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/dark.css"> 
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/pure.css"> 
<link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify/themes/dolphin.css">
```

### 4.2. 封面
coverpage设置为true, 就可以打开封面功能

然后在项目文件夹, 新建文件`_coverpage.md` 即为封面. 文件内容如下:
```

# Sugar的知识库 


**Author**: Sugar

[**Start**](README.md)

```


### 4.3. 导航栏
loadNavbar设置为true, 在项目文件夹中添加`_navbar.md`文件, 即为导航栏, 文件内容如下: 
```
- 学习笔记
    - 正在整理中
    - 正在整理中
    - 正在整理中
    - 正在整理中

- 云端部署
    - [docker搭建思源笔记](docker搭建思源笔记.md)
    - [docsify搭建记录](docsify搭建记录.md)
    - [docker笔记](docker笔记.md)

- 工具网站
    - [正则表达式30分钟入门](https://deerchao.cn/tutorials/regex/regex.htm)
    - [Hex字符转换](https://the-x.cn/encodings/Hex.aspx)
    - [MathSolver](https://mathsolver.microsoft.com/zh)
    - [TextTools](https://mytexttools.com/)
```
### 4.4. 侧边栏
默认情况下, 会根据但钱文档的标题生成侧边栏目录. 

也可以自己定制化侧边栏的内容, 修改index.html文件
```html
<script> window.$docsify = {
    loadSidebar: true
  } </script>
<script src="//unpkg.com/docsify"></script>
```

设置loadSidebar: true之后, 在项目的根目录创建`_sidebar.md`文件, 内容格式如下:
```
* [home1](home1)
* [home2](home2)
* [bar](bar/)
* [bar/a](bar/a)
```

**显示页面目录**
定制的侧边栏, 只会显示页面连接, 如果想同时显示页面连接和当前文档的目录, 就需要添加`subMaxLevel`字段.
```html
<script>
  window.$docsify = {
    loadSidebar: true,
    subMaxLevel: 3
  }
</script>
<script src="//unpkg.com/docsify"></script>
```


## 5. 部署
> 使用docker镜像为最优选择, 使用nginx是在不了解docsify的情况下进行的尝试  
> 使用nginx有点臃肿, 需要自己搭建web服务  
> 使用docsify自带的serve服务更为简单便捷

可选下列三种方式进行部署:
1. 使用 nginx 
2. 使用 [github pages](https://docsify.js.org/#/zh-cn/deploy?id=github-pages) 
3. 制作 [docker 镜像](https://docsify.js.org/#/zh-cn/deploy?id=docker)

### 5.1. 使用docker镜像进行部署
> 选用 docker 镜像部署比较方便, 这里只讲解此种方式  
> docsify没有官方的镜像, 需要使用Dockfile自己制作镜像并运行. 

1. 首先准备好初始文件
	```
	index.html
	README.md
	```
	
2. 创建`Dockfile`
	```
	FROM node:latest
	MAINTAINER sugar<1144392179@qq.com>
	WORKDIR /docs
	RUN npm install -g docsify-cli@latest
	EXPOSE 3344/tcp   # 此处暴露的是主机端口
	ENTRYPOINT docsify serve .
	```
3. 构建镜像
	```
	docker build -f Dockfile -t sugar/docsify .
	```
4. 运行docker镜像
	```
	docker run -it -p 3344:3000 --name=docsify -v $(pwd):/docs sugar/docsify
	```

## 6. reference
1. [docsify官方文档](https://docsify.js.org/#/)
2. [思否-docsify教程](https://segmentfault.com/a/1190000017576714)


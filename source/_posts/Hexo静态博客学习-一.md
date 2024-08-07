---
title: Hexo静态博客学习(一)
tags: hexo
layout: 
date: 2024-08-07 18:02:33
---


## 博客搭建
使用npm下载即可，以后补充

## 基础命令

```bash
hexo cl 或 hexo clean      //清空public文件夹内容
hexo g 或  hexo generate   //在hexo站点根目录下生成public文件夹，相关静态网页文件
hexo s 或  hexo server     //启动服务预览 http://localhost:4000
hexo d 或  hexo deploy     //部署站点，在本地生成.deploy_git文件夹，并将编译后的文件上传至 Git远程仓库，如github或自己搭建的远程服务器。
```

## 项目文件结构

```log
+----- /.deploy_git  # 部署git的本地仓库
|
+----- /.git         # git相关文件
|
+----- /node_modules  # 安装插件时存放插件的目录
|
+----- /public       # 执行 hexo cl 会删除该文件夹，执行hexo g 会生成该文件，hexo s 启动也是将本目录作为本地服务器目录
|
+----- /source       # md 文章页面文件，一般自定义页面会在这里放
|    |
|    +----- /_posts  # 自己放文章的目录，内部目录结构随便创建
|    |
|    +----- /about  # 关于页面
|    |
|    +----- /categories  # 分类页面
|    |
|    +----- /friends  # 友链页面
|    |
|    +----- /search  # 搜索页面
|    |
|    +----- /tags  # 标签页面
|    |
|    +----- 其他页面，就不一一列举了
|
+----- /themes      # 主题存放目录
|
+-----+----- /landscape # 默认下载的主题
|      |
|     +----- /hexo-theme-matery  # 你自己下载的其他主题目录，我的是matery
|          |
|          +----- /languages  # 主题的语言支持目录
|          |
|          +----- /layout   # 主题的模板布局目录，ejs、pug等模板文件之类的等
|          |
|          +----- /scripts  # 主题渲染相关脚本相关，一般不需要改动
|          |
|          +----- /source   # 主题的一些静态资源文件夹，如css样式，js文件，图片文件等
|          |    |
|          |    +----- /css  # 主题页面使用的相关样式文件
|          |    |
|          |    +----- /js   # 主题页面使用的相关脚本，
|          |    |
|          |    +----- /images   # 主题相关图片文件
|          |    |
|          |    +----- /libs  # 主题相关第三方插件
|          |    |
|          |    +----- 其他名字的目录或者文件 
|          |
|          +----- _config.yml  # 主题配置文件，非常重要
|
+----- _config.yml  # hexo 根目录配置文件，非常重要
|
+----- package.json  # hexo 安装插件的描述文件，比较重要，如果你换目录，换电脑，有这个文件就可以直接安装之前安装过的插件。

```



## 问题解决
### 生成链接错误
![](https://raw.githubusercontent.com/sustcsugar/picgo/main/img/202408071823773.png)


## reference
[Hexo搭建静态博客（一）——基础搭建](https://small-rose.github.io/posts/9f117b.html)
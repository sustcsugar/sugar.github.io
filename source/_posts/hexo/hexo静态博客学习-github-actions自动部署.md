---
tags:
  - hexo
title: hexo静态博客学习-github-actions自动部署
date created: 20240808223136
date modified: 20240809002554
categories: 文字工作
abbrlink: d2305d95
date: 2024-08-09 00:27:06
aliases:
status:
---

## 介绍

利用Github Actions，我们就能建立两个代码仓库，比如：
- `blog_src`：私有，保存源代码，内有个人数据
- `blog.github.io`：公开，大家访问的资源

然后借助Github Actions关联起来, 将文章推送到`blog_src`之后, 触发`action`自动生成新的静态页面, 访问`github.io`即可.




## reference

1. https://blog.csdn.net/lpl0223/article/details/137436353
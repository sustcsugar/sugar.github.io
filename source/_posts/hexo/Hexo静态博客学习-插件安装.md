---
title: Hexo静态博客学习-插件安装
abbrlink: e989a385
tags:
  - hexo
categories: 文字工作
date: 2024-08-07 23:09:24
date_created: 20240807_230924
date_modified: 20240809_002554
---


## 常用插件

> 列出所安装的插件
> 插件的使用和配置在文章后面会逐个说明

```bash
npm install hexo-abbrlink --save          # 永久链接,修改文章url格式
npm install hexo-reading-time --save      # 显示阅读时长
npm i --save hexo-wordcount               # 文章字数+阅读时长统计
npm install hexo-generator-search --save  # 文章搜索插件

```

## 永久链接配置
> 在生成的 abbrlink 前面添加时间戳，增加可读性
> [abbrlink官方](https://github.com/ohroy/hexo-abbrlink)
```yml
# permalink: :year/:month/:day/:title/
# permalink_defaults:
permalink: posts/:year:month:day:hour:minute:second-:abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```

## 显示阅读时长
> matery主题中自带了该配置，只需要按照注释下载对应的插件即可
```yml
# Post word count, reading duration, site total word count.
# Before you activate, please confirm that you have installed the hexo-wordcount plugin,
# install the plugin command: `npm i --save hexo-wordcount`.
# 文章字数统计、阅读时长、总字数统计等
# 文章信息--若要开启文章字数统计，需要安装 hexo-wordcount 插件，安装命令: `npm i --save hexo-wordcount`
postInfo:
  date: true # 发布日期
  update: false # 更新日期
  wordCount: true # 文章字数统计
  totalCount: false # 站点总文章字数
  min2read: true # 文章阅读时长
  readCount: false # 文章阅读次数
```

## 文章搜索功能
修改根目录的 `_config.yml`

```yml
search:
  path: search.xml
  field: post
```
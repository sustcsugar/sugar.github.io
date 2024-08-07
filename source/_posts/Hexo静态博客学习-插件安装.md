---
title: Hexo静态博客学习-插件安装
tags: hexo
abbrlink: 22479
date: 2024-08-07 23:09:24
categories: 技术文章
---


## 常用插件

```bash
npm install hexo-abbrlink --save            # 永久链接,修改文章url格式
npm install   hexo-reading-time --save      # 显示阅读时长
```

## 永久链接配置
```
# permalink: :year/:month/:day/:title/
# permalink_defaults:
permalink: posts/:abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```
---
title: Hexo静态博客学习-matery主题优化
abbrlink: dfacf917
date: 2024-08-08 00:36:17
tags: hexo
categories: 技术文章
---

## 首页颜色变化

修改文件 : `/source/css/matery.css`, 搜索 `rainbow` 来关闭颜色变换：
```css
@-webkit-keyframes rainbow {
   /* 动态切换背景颜色.即滤镜颜色，不想要可以全部注释，或者换成你喜欢的颜色 */
}

@keyframes rainbow {
    /* 动态切换背景颜色.，不想要可以全部注释，或者换成你喜欢的颜色 */
}
```

## 代码块修改

> `hexo + matery` 自带的代码块渲染后出问题
> 1. 高亮效果太差了，不好看。
> 2. 格式渲染也有问题。
![](https://raw.githubusercontent.com/sustcsugar/picgo/main/img/202408081052616.png)

安装插件： `npm i -S hexo-prism-plugin`

按照下面步骤修改 `config` 文件

```yml
# 根目录的 _config.yml
syntax_highlighter: highlight.js
highlight:
  enable: false
  line_number: false
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs:
  enable: true
  preprocess: true
  line_number: true
  tab_replace: ''
```

```yml
# matery theme 中的 _config.yml
# 代码块相关
code:
  lang: true   # 代码块是否显示名称
  copy: true   # 代码块是否可复制
  shrink: true # 代码块是否可以收缩
  break: true  # 代码是否折行
```

修改`css`设置：`themes\source\css\matery.css`


## 更换字体

## 样板
1. https://small-rose.github.io/
2. https://marmalade.vip/

## ref
1. [Matery主题新手常见问题](https://small-rose.github.io/posts/a53a9069.html)
2. [Matery之代码块优化](https://cloud.tencent.com/developer/article/2148822)
3. [matery主题的代码块问题解决](https://www.rewind.show/2020/12/23/BUG%E5%A4%84%E7%90%86/matery%E4%B8%BB%E9%A2%98%E7%9A%84%E4%BB%A3%E7%A0%81%E5%9D%97%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/)
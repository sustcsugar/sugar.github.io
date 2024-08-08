---
title: Hexo静态博客学习-matery主题美化
abbrlink: dfacf917
date: 2024-08-08 00:36:17
tags:
  - hexo
categories: 技术文章
---

## 1. 首页颜色变化

修改文件 : `/source/css/matery.css`, 搜索 `rainbow` 来关闭颜色变换：
```css
@-webkit-keyframes rainbow {
   /* 动态切换背景颜色.即滤镜颜色，不想要可以全部注释，或者换成你喜欢的颜色 */
}

@keyframes rainbow {
    /* 动态切换背景颜色.，不想要可以全部注释，或者换成你喜欢的颜色 */
}
```

## 2. 导航栏颜色

修改`matery.css`文件`.bg-color`选择器

```css
.bg-color {
    background-image: linear-gradient(to right, #e657ce 0%, #1059e0 100%);
}
```


## 3. 卡片区背景

修改`matery.css`文件`body`选择器

```css
body { 
    background-color: #eaeaea; 
    background: linear-gradient(60deg, rgba(224,255,125, 0.5) 5%, rgba(0, 228, 255, 0.35)) 0% 0% / cover;
    background-attachment: fixed; 
    margin: 0; 
    color: #34495e; 
}
```



## 4. 代码块修改

> `hexo + matery` 自带的代码块渲染后出问题
> 1.  高亮效果太差了，不好看。
> 2. 格式渲染也有问题。
![](https://raw.githubusercontent.com/sustcsugar/picgo/main/img/202408081052616.png)

### 4.1. 代码高亮

按照下面步骤修改根目录的 `config` 文件

```yml
# 根目录的 _config.yml

# hexo 版本 7.0.0以上时,需要设置 highlighter 选项
# 这里需要注意,要把 highlighter 修改为 prismjs (注意不是prism.js)


syntax_highlighter: prismjs
highlight:
  enable: false
  line_number: false
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
# hexo 官方支持的prism
prismjs:
  enable: true
  preprocess: true
  line_number: false
  tab_replace: ''

# prism 插件
# 由于 hexo 官方已经支持了 prism ,所以就不需要安装 prism-plugin 了,如果已经安装了,卸载即可
# 安装插件 npm i -S hexo-prism-plugin
# 卸载插件 npm uninstall hexo-prism-plugin
prism_plugin:
  enable: false
  mode: 'preprocess'    # realtime/preprocess
  theme: 'tomorrow'
  line_number: true    # default false
  custom_css: 
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

### 4.2. 代码行号

设置完之后发现代码块不显示行号,参考[这篇文章](https://blog.csdn.net/weixin_45453133/article/details/120853394)发现是`matery`对`prism.css`的适配出问题了,需要修改主题中的`prism.css`文件.

按照这个方法修改`prism.css`文件之后, 显示行号了,但是选择代码时后出现错位问题. 这个问题现在还没有解决.
![](https://raw.githubusercontent.com/sustcsugar/picgo/main/img/202408081608464.png)


### 4.3. 代码折行

## 5. 更换字体

> 更换字体一个很麻烦的问题是,使用本地字体时,打开网页需要加载字体. 如果字体文件较大,会十分影响网页的加载速度.
> 
> 因此可以进行取舍, 使用大多数设备都支持的本地预装字体. 或者使用web字体.


### 5.1. 全局更换字体

在`hexo`根目录下创建并添加字体文件`/source/font/myfont.ttf`.

然后进入主题文件夹`/themes/hexo-theme-matery/source/css/my.css`, 在其中添加下面的`css`片段用来控制全局字体.

```css

@font-face{
    font-family: 'myFont';
    src: url('../font/myFont.ttf');
}

body{
    font-family: 'myFont';
}

```

### 5.2. 局部更换字体

和全局字体类似, 通过网页审查工具,找到想要修改字体的网页元素,然后修改`my.css`文件来修改字体.

```css
@font-face{
    font-family: 'navFont';
    src: url('../font/jianqiti.ttf');
}


nav {
    font-family: 'navFont';
}
```

### 5.3. web安全字体

> 使用广泛支持的“Web安全”字体可以显著减少网页加载时间，因为这些字体通常已经安装在大多数用户的系统上，不需要额外下载。

```css
body {
    font-family: "Songti SC", SimSun,"宋体","仿宋", "微软雅黑","黑体", Arial, Helvetica, sans-serif;
}
```


### 5.4. 精简字体

如果某些字符,在网站中只使用了几次, 例如标题, 导航栏等等.

那么我们可以将完整的字体文件中,相对应的字符提取出来, 打包成一个新的`ttf`文件. 这样可以在美化网站的同时, 降低资源用量.  

https://cyh.me/posts/font-minification/


## 6. 样板
1. https://small-rose.github.io/
2. https://marmalade.vip/
3. https://zahui.fan/

## 7. reference
1. [Matery主题新手常见问题](https://small-rose.github.io/posts/a53a9069.html)
2. [Matery之代码块优化](https://cloud.tencent.com/developer/article/2148822)
3. [matery主题的代码块问题解决](https://www.rewind.show/2020/12/23/BUG%E5%A4%84%E7%90%86/matery%E4%B8%BB%E9%A2%98%E7%9A%84%E4%BB%A3%E7%A0%81%E5%9D%97%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/)
4. [Hexo官方教程-语法高亮](https://hexo.io/zh-cn/docs/syntax-highlight#PrismJS)
5. [Marmalade's Blog Hexo-Matery主题细致美化(下)](https://marmalade.vip/Materysettings2.html#toc-heading-8)
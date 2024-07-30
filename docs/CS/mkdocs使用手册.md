
> 作者: sugar
> 
> 转载请联系作者(shigang-97@qq.com ),并说明出处!


## 1. 概述

## 2. 安装
需要 python 环境，使用 pip 安装.`pip install mkdocs`
安装完之后，可以使用 `which mkdocs` 检查.

## 3. 开始

### 3.1. 创建新项目

```bash
mkdocs new my-project
cd my-project
```

有一个配置文件 `mkdocs.yml`, 和一个包含文档源码的 `docs` 文件夹. 在 `docs` 文件夹里包含了一个名为 `index.md` 的文档.

```
~/source/repos/work/my-proj $ tree -C
.
|-- docs
|   `-- index.md
`-- mkdocs.yml

```




## 4. 配置
配置文件为 `yml` 格式的,语法需要注意:

- nav 中的文件名,需要**双引号**, 标签名不需要引号.

- markdown_extentions 中的冒号,需要空格

```yml
site_name: Sugar's Docs
nav:
  - "index.md"
  - Docsify:
    - "docsify搭建记录.md"
markdown_extentions:
  - toc:
    permalink: True
  - footnotes

```

## 5. 主题配置

在`mkdocs.yml`中添加主题配置，可以修改主题。 `material`为[第三方主题](https://squidfunk.github.io/mkdocs-material/)，需要额外安装。

```yml
theme: 
  name: material
```

## 5. Reference
1. [MkDocs 中文文档 (markdown-docs-zh.readthedocs.io)](https://markdown-docs-zh.readthedocs.io/zh_CN/latest/)
2. [笔记文档一把梭——MkDocs 快速上手指南 ｜ 少数派会员 π+Prime (sspai.com)](https://sspai.com/prime/story/mkdocs-primer)
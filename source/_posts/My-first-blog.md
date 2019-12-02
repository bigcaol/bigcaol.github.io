---
title: My first blog!
date: 2018-10-25 17:21:19
tags: 
    -hexo
categories: 后端
description:
---

[TOC]

## 前言

今天在自己win10上搭建了Hexo博客，这是我第一篇博客，所以主要目的仅仅是验证测试。之前自己也在自己的服务器上搭建了wordpress，虽然搭建不是很难，而且搭建后几乎不会像Hexo这样使用命令行控制，但是wordpress使用中感觉很臃肿，markdown编辑支持也不是很好。

## 参照网站
贴出主要的参照网站，方便以后维护。
搭建过程主要参照[这篇文章：从零开始使用 Hexo 搭建博客](https://www.dingxuewen.com/2017/05/15/reprinted-how-to-build-a-blog-with-hexo/)
使用主题为[Next](http://theme-next.iissnan.com/),官网中有该主题的具体美化方法，包括字体字号、打赏功能等等。介绍清晰，感兴趣的可以看看。
[hexo官网](https://hexo.io/zh-cn/)
## 优势
1. 简单，该有的功能都有，专注于写内容。
2. 基于github，所以不担心丢失，每个版本都能恢复。
3. 基于github，不占用自己的服务器。
#基于github，所以不是私密的，所以大家都能搜索到。

## 使用方法

常见命令

### 新建文章

```bash
hexo new "name_of_articl"	#新建文章
hexo n "name_of_articl"		#缩写
hexo new blog "name_of_articl"	#使用blog模板
```

### 生成静态页面至public

```bash
hexo clean	#最好先清除缓存
hexo g == hexo generate	#生成静态页面至public目录
```

### 本地预览

```bash
hexo s == hexo generate	##本地预览，浏览器打开http://localhost:4000即可
```

### 上传github

```bash
hexo d == hexo deploy	#上传至github
```

### 组合命令

```bash
hexo s -g	#生成并本地预览
hexo d -g	#生成并上传
```

更多的命令参见[hexo官网](https://hexo.io/zh-cn/)


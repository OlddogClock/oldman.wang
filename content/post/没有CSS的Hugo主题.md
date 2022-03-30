---
title: "没有CSS的Hugo主题"
date: 2022-03-30T15:35:38+08:00
description: ""
draft: false
categories:
  - 程序员杂志
tags:
  - Hugo
  - Hugo Themes
---

这是一个没有CSS的Hugo博客主题

<!--more-->

项目地址：https://github.com/OlddogClock/nocss-hugo


* 仅支持首页、分类列表、文章详情页
* 图标使用[emoji](https://www.emojiall.com/)
* 支持插入哔哩哔哩视频
  * {{`< b BV1q5411S7Uf >`}}

但是，由于各种各样的原因，还是要有一些CSS的

目前仅有的`css/alone.css`
* 限制图片尺寸
* B站视频
* 第三方插件
  * highlight.js

正在根据实际使用情况考虑要不要加入github-markdown-css

开发环境：

```shell
hugo version
hugo v0.93.0-07469082+extended windows/amd64 BuildDate=2022-02-28T08:30:42Z VendorInfo=gohugoio
```

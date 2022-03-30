---
title: Windows10设置Path环境变量的简单方法
categories: 程序员杂志
tags:
  - Windows 10
date: 2022-01-02
---
给小白看的，人问起来，写下来吧，Win10的系统属性藏的还是比较深的。
<!--more-->
方法一、用`Win`+`R`快捷键，调出`运行`对话框后，输入`sysdm.cpl`，按照以下红色箭头操作，点击`Path`，在绿色框内，输入你要添加的值，比如`d:\gstreamer\1.0\x86_64\bin`
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/设置环境变量.jpg)

方法二、用`Win`+`R`快捷键，调出`运行`对话框后，输入`cmd`，不要点确定，按`Ctrl+Shift+Enter`快捷键，弹出管理员模式的命令行

![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/管理员cmd.jpg)

比如要新增`d:\gstreamer\1.0\x86_64\bin`，就输入

```
setx "Path" "%PATH%;d:\gstreamer\1.0\x86_64\bin" /m
```

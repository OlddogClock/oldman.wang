---
title: ES版CPU还真有点香
categories: 程序员杂志
tags:
  - CPU
  - DIY
date: 2022-02-15
---
🐒ES，Engineering Samples工程样品，是Intel在产品上市之前发给主板厂商、媒体等机构的测试样品，在CPU-Z里不显示CPU型号、主频比正式版低，也有可以正确显示型号的，名称后面会带ES标示，正显也被称为QS版（Qualification Sample品质确认样品），更多介绍可以执行查询。圈里叫我们这些用ES版CPU和淘汰Xeon的人是”垃圾佬“，十分贴切
<!--more-->
🌈理论上讲ES版CPU不稳定，但经过我的长时间测试和使用，没有任何不稳定的现象，我虽然不玩有戏，但是FFmpeg、TensorFlow、OpenCV、MxNet等都是很耗费CPU的，满载运行一两个小时甚至一天是常有的事。

💰这次买到的CPU是Intel i9 10900 ES QTB1不显版（CPU-Z和Windows中显示的是Intel 0000），热设计功耗65W，是我的这块H410主板支持到的最高型号，10900K、KF都无法支持，因为这是一块mini ITX结构使用19V直流供电的主板，也不支持独立显卡。之前的一年一直使用i5 10400 ES正显 6核心12线程，*本次花费1400元直接到顶i9 10900 10核心20线程，比正式版便宜了一千多*，配置如下：

💻
CPU：Intel i9 10900 ES QTB1 不显版
主板：Asus PRIME H410T2/CSM（直流19V供电）
内存：金士顿 骇客神条 DDR4 2600MHz 16G
显卡：（无）
硬盘：WD蓝盘 500GB M.2
操作系统：Windows 11 企业版

🏠总体使用下来，没有出现任何不稳定的情况，跑分不高，毕竟是低功耗主机；
⚡️睿频可以到4.2G，但是由于主板供电不给力，满载运行时会掉频到2.5G；
🎬压缩视频时的效率有了明显提升，一部12G两小时的电影使用HandBrake默认配置压缩仅需要不到1小时即可完成，原来i5 10400需要90分钟
🌡满载一小时，温度不超过70℃

### Intel i5 10400 正显版
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/IMG_5395.jpeg)
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/IMG_5396.jpeg)
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/IMG_5398.jpeg)
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/IMG_5399.jpeg)

### Intel i9 10900 ES 不显版
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/20220218111130.png)
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/IMG_5437.jpeg)
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/IMG_5438.jpeg)
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/IMG_5439.jpeg)
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/IMG_5440.jpeg)
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/20220216154855.png)
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/20220216155122.png)

![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/IMG_5435.jpeg)
---
title: 网件R6800刷PandoraBox固件
categories: 程序员杂志
tags:
  - 路由器
  - 原创
date: 2022-01-02
---

Netgear R6800是我2017年8月购买的路由器，个头非常大，尺寸与MacBook差不多，一直以原厂固件稳定运行，从来没出过问题，直到WiFi6路由器上市后才换掉它。网件R6800硬件上是支持160MHz宽频的，但是被软件锁住了，智能到80MHz，刷PandoraBox可以解决这个问题。160MHz宽频可以让无线网卡5G信号速率达到1733Mbps，2.4G信号也可以达到800Mbps（原厂只有600Mbps），发挥该老路由器的最佳性能，而且过程非常简单。
<!--more-->
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/7A6816C6-2E82-403D-A88E-32A4150C6377.png)

1. 在[恩山无线论坛](https://www.right.com.cn/forum/thread-494214-1-1.html)下载[固件镜像](https://downloads.pangubox.com/%E5%88%B7%E6%9C%BA%E8%AF%B4%E6%98%8E/R6950/PandoraBox-r6700v2-r6800-r6900v2-factory-web.img)，下载不下来就用迅雷或者联系我。
2. 在高级->管理->固件更新-> 从您的硬盘上找到并选中升级文件`PandoraBox-r6700v2-r6800-r6900v2-factory-web.img`升级即可，如果你的原厂固件比较新，会有提示让你确认是否退回老版本，选择是即可。注：刷固件会导致所有的路由器设置丢失；变砖的话看上面的帖子自救。
3. 然后就是等待...重启之后进入管理地址： http://192.168.1.1 用户root,密码admin
4. 设置就很简单了，我使用的是2.4G和5G信号分开的，信道要选低一些的。
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/B085278A-DF23-4767-8F6A-611F1665A228.jpg)
5. 由于我是在远程天文台使用，关闭了所有的LED灯
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/关闭路由器LED.png)
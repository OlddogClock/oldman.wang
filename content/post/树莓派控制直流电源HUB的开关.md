---
title: 树莓派控制直流电源HUB的开关
categories: 程序员杂志
tags:
  - 树莓派

date: 2021-12-21 00:00:00
---

我电子相关知识基本为0，停留在高中物理阶段，现用现查吧，欢迎指正。
这是一个简易版，好听点叫”原型“，直白点说就很简陋，只能在自己的局域网内、自己一个人使用。
<!--more-->
但我没有使用这个方案来控制电源HUB，采用了RS232串口控制，直接在Windows主控电脑上，这样就免去了树莓派。
如果你对此方案有意，可以随时问我（微信号: `qi--_--ip__olo`），我会将此方案进行完善，比如可以通过互联网远程控制开关等

## 目的
通过网页开关12V电源HUB上的单个接口。主要是给远程天文望远镜用。

## 优点
- 树莓派自带wifi，能少拉一根网线
- 可以利用自己熟悉的高级语言编程，比如我用的Nodejs

## 不足
- 无法记录断电前状态（可以实现，但是我没做）
- GPIO连接会有一堆杜邦线
- 体积还是比较大的
- 完成版比较丑，我就不拍照了

## 材料
* 树莓派3B+。这东西涨价真是太厉害了，我是以高于当年购买全新4B4G的钱买了一个二手3B+
* [光耦继电器](https://item.taobao.com/item.htm?id=570578720722)，我使用的是4路，触发信号3.3V~5V，供电电压12V。树莓派的GPIO不能输出超过5V的电平，所以触发信号不要选错了。耐受电流10A，掌握好用电设备功率。
* 杜邦线、防水盒子、DC55-21母头插座、0.25和0.75平方铁氟龙电线

继电器接线示意图：
![](https://img.alicdn.com/imgextra/i2/2495360764/O1CN01wHOEBZ1HVxHhm1oPL_!!2495360764.jpg)
![](http://oldmanblog.oss-cn-guangzhou.aliyuncs.com/blog/树莓派继电器.jpg)

## 过程
源代码：https://opensourcedamnbright.coding.net/p/cac/d/power-hub-pi/git ，`myswitcher.d`在`/etc/init.d`目录下实现开机启动等服务管理

树莓派系统是bullseye，无桌面的Lite版，自带Python 3.9（后来退回到了2.7），Nodejs 16，配置详细过程略。使用`onoff`（https://github.com/fivdi/onoff）控制GPIO，`git clone`下来后进入目录，执行`npm install`如果安装过程中出现问题，就将Python3换回python2.7，再重装`node-gyp`，然后再运行`node-gyp rebuild`，不管是否出错，onoff可用了能跑起来就OK
```
npm uninstall node-gyp
sudo npm uninstall node-gyp -g
npm install node-gyp
sudo npm install node-gyp -g

```

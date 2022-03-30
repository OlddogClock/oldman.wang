---
title: GTX3060成功安装tensorflow-gpu过程记录
categories: 程序员杂志
tags:
  - gtx6060
  - tensorflow
  - GPU
  - 原创
date: 2021-11-06 00:00:00
---

Windows10下成功安装Tensorflow-GPU过程（GTX 3060）

本文所述事情发生在2021年11月6日，特意强调时间是因为看过一些旧的教程，踩过坑。

重点是显卡驱动、CUDA、cudnn、tensorflow-gpu的版本应对。tensorflow的安装文档基本不用看
<!--more-->
## 显卡和系统基本情况

* GIGABYTE RTX 3060 GAMING OC Rev. 2.0（GV-N3060GAMING OC-12GD）技嘉魔鹰2.0显卡，这是块锁算力的卡；
* Windows 10 企业版64bit 21H1，19043.1320
* Anaconda3，python3.7，tensorflow-gpu-2.7.0
* 显卡驱动GameReady 471.41，CUDA v11.2.2，cudnn v8.1.1
* 其他配置：
  * CPU：AMD Ryzen 7 3700X 
  * 主板：Gigabyte B550I Aorus Pro AX
  * 内存：金士顿 DDR4 3200MHz 16G

![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/gpu2.gif)

## 安装步骤

0. 分为"确定版本信息、下载安装cuda、设置环境变量、安装tf"，四个大步骤

### 确定版本信息

1. 安装显卡驱动，我用的GameReady版，最高版本是496.xx，没追新，用的是471.41；
2. 确认自己能安装什么版本的CUDA。打开"NVIDA控制面板"-->帮助-->系统信息-->查看NVCUDA64.DLL后面对应的NVIDIA CUDA版本；
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/显卡控制面板.PNG)
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/驱动对应.PNG)

3. Nvidia GeForce GameReady 471.41驱动最高支持到CUDA v11.4，但是Tensorflow目前不支持这么高的CUDA版本，所以我根据tf文档中的提示，安装tensorflow_gpu-2.6.0 对应的CUDA 11.2
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/tf-cuda版本对应.PNG)

### 下载安装

4. 在`https://developer.nvidia.com/cuda-toolkit-archive`下载CUDA Toolkit 11.2 Update 2，稍后安装
5. 在`https://developer.nvidia.com/rdp/cudnn-archive`下载cuDNN v8.1.1 (Feburary 26th, 2021), for CUDA 11.0,11.1 and 11.2，稍后安装
6. 在`https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/`下载Anaconda3-2021.05-Windows-x86_64.exe，直接安装
7. 安装CUDA，选择自定义安装，我只勾选了CUDA->Runtime-Libraries，其它都不安装，因为第1步安装驱动时包含了一部分，VisualStudio集成等功能tf也用不到；使用默认安装路径，我的在C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/cudasetup.PNG)
8. 将第5步下载的cudnn-11.2-windows-x64-v8.1.1.33.zip\cuda\bin\下所有文件复制到CUDA目录C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\bin\
9. 再将cudnn-11.2-windows-x64-v8.1.1.33.zip\cuda\lib\x64 下所有文件复制到C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\lib\x64\

### 设置环境变量

10. 在系统变量中，CUDA安装程序会自动增加`CUDA_PATH`和`CUDA_PATH_V11_2`变量取值为安装目录，无需人为干预
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/环境变量1.PNG)

11. 修改系统变量的Path变量，在最前面增加`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\bin;C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\lib\x64;`两个目录
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/环境变量2.PNG)

12. 在系统变量中，增加变量名`TF_GEFORCE_GPU_ALLOW_GROWTH`，取值为true。不知干嘛的，从别人教程里搬过来的
13. 重启

### 安装tensorflow_gpu
14. 创建一个tf环境`conda create -n mytf python=3.7`。我之前失败的尝试都是py3.8，这次尝试使用py3.7成功了，不知道py版本是否有关系，按照tf官方文档，3.8是可以的
15. 进入tf环境`conda activate mytf`；我以为现在tf的最高版本是2.6.0，上面的安装也都是照着tf2.6.0来的，所以安装tf时没有带版本号，直接`pip install tensorflow-gpu -i https://pypi.doubanio.com/simple/`，结果发现安装的是2.7.0
16. 测试代码如下
```
import tensorflow as tf
print('GPU',tf.config.list_physical_devices('GPU'))
print("Num GPUs Available: ", len(tf.config.experimental.list_physical_devices('GPU')))
```
我只有一块GTX3060显卡，执行后返回:
```
GPU [PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
Num GPUs Available:  1
```
到此为安装成功。

## 失败的情况
CUDA11.1＋cuDNN8.0.5＋tensorflow-gpu2.4.1，照别人3060安装成功的教程搬过来的，在我这失败，提示cusolver64_10.dll丢失，当时用的Python3.8，显卡驱动496.xx

## 为Windows Terminal增加Anaconda prompt
Windows Terminal是Win10下很好用的一个终端，但是默认无法显示出conda的环境名称，可以手工增加：
打开Windows Terminal设置，再左下角的"打开JSON文件"

![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/wt.PNG)

profiles.list数组增加如下代码后，重新打开Windows Terminal即可：
```
{
    "bellStyle": 
    [
        "window",
        "taskbar"
    ],

    //将Anaconda Powershell Prompt (Anaconda3)快捷方式属性中的"目标"复制到commandline字段中，\和"符号前要加\避免被转义
    "commandline": "%windir%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe -ExecutionPolicy ByPass -NoExit -Command \"& 'C:\\ProgramData\\Anaconda3\\shell\\condabin\\conda-hook.ps1' ; conda activate 'C:\\ProgramData\\Anaconda3' \"",  

    //自己生成一个guid，用下面的也行，只要和上面不重复即可
    "guid": "{45b48db0-01a4-4d92-937b-60f9928e8c5c}",
    "hidden": false,

    //找个好看的icon
    "icon": "C:\\ProgramData\\Anaconda3\\Menu\\anaconda-navigator.ico",

    //Windows Terminal菜单中显示的名称
    "name": "Anaconda Prompt"
}
``` 
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/wt2.PNG)


## 为什么任务管理器中没有CUDA图表
安装完CUDA后，Windows任务管理器-->性能-->GPU图表里会有CUDA这一选项，
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/640.png)

这个选项并不能判断CUDA是否安装成功，如果开启了Windows设置--显示-图形设置--硬件加速，则任务管理器中的CUDA选项会消失
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/task2.PNG)
![](https://oldmanblog.oss-cn-guangzhou.aliyuncs.com/task1.PNG)


---
layout: post
title:  "在 Ubuntu 用UVC支持 使用 WebCam摄像头传感器"
date: 2015-08-27 18:22:07 +0800
tags: 
slug: p20150827182207
---

# 在 Ubuntu 用UVC支持 使用 WebCam摄像头传感器





## 在 [Ubuntu](https://so.csdn.net/so/search?q=Ubuntu&spm=1001.2101.3001.7020) 用UVC支持 使用 WebCam摄像头传感器


  

**写在前面：**
  

* 这里我为了省事，用了虚拟机。但注意，虚拟机下有的WebCam驱动程序可能不支持！
* '我的宿主机是Win 7系统。虚拟机工具VirtualBox 4.3.28. 虚拟系统：Ubuntu 14.04 LTS
* 'VirtualBox上运行的Ubuntu系统，要使用摄像头，需打通VirtualBox对USB设备的支持，要安装对应的Oracle VM virtualBox Extension Pack（Google 搜索）
* '关于Ubuntu/Linux系统中cheese工具支持的摄像头类型，有一个叫做UVC的组织，提供了一套[Linux UVC driver](http://www.ideasonboard.org/uvc/)支持大部分型号的WebCam
* 本文使用的WebCam型号： Logitech HD Webcam C525


  

**获取图像：**
  
    参考： 
[ubuntu12.04+virtualbox+winxp的关于摄像头无法使用，声音出不来的问题](http://www.cnblogs.com/etangyushan/p/3679440.html)
  

  

**安装cheese以测试:**
  
     $ sudo apt-get install cheese 
  

    或者通过 ubuntu 图形界面 软件app中心 搜索 cheese 进行 Install。  
 


**测试运行cheese:**


   $  cheese


   应该能看到图像了~~  
    说明： cheese 或 guvcview 这样的 webcam on linux 工具都是内含 UVC Driver 的。  
 


     
 

    
  




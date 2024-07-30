---
layout: post
title:  "虚拟机上的Linux配置tftp服务器与下载uImage到mini2440开发板成功经验"
date: 2014-05-21 22:06:23 +0800
tags: 
slug: p20140521220623
---

# 虚拟机上的Linux配置tftp服务器与下载uImage到mini2440开发板成功经验






```
我配tftp想下载u-boot.bin到NandFlash。我的Fedora14是跑在win7 PC 上的virtualbox虚拟机上的。win7PC与开发板通过一根网线直连。此时问题出来了：我不明白Fedora14怎么通过VBox使用win7的网卡继而与开发板建立tftp服务。不明白网络相关的知识，有没有相关的教材？谢谢！
```


```
不是请问简单的tftp的下载，配置，启动等问题，而是想知道如何设置ip等网络参数，使得tftp服务能通。

一些进展：
    1. 首先要明白什么是virtualbox或者VMWare中的网络设置：bridge/NAT/Internal/Host-only，这个在“<http://blog.csdn.net/mrjy1475726263/article/details/7772372>”有明确的说明。这里我们使用了bridge连接方式。
    2. 然后我开发板linux Root File System起来以后，Host Linux设置好了IP，具体参照的是“<http://jingyan.baidu.com/article/455a99508be7cda167277865.html>”中的说明进行的设置。

    3. 物理连接：PC-网线-开发板，直连。 此时ping 开发板ip，已经显示能ping通。【空了配一个开发板linux下的ftp连接试试，不玩uboot】
```


```
		如何安装、配置和设置ftp server在HostLinuxPC上，参考本文： <http://blog.sina.com.cn/s/blog_696088df0100lbt4.html>

```


```
		如何设置网卡成自动获取ip，静态ip等：<http://zhidao.baidu.com/link?url=FGR4oUOlw8fkooLamD49m3_aBQCcW5jX4g46_tJSb0bJO2FWieZbFjq6gMLkG1Is7-LZBxw-d6Wjey28k7lMZa>
```


```
		Fedora 安装、配置、设置ftp server可参考：<http://blog.csdn.net/jdh99/article/details/7217478>
```


```
		装好vsftpd服务以后，用命令 #/sbin/service vsftpd start开启服务。
```



```
    4. 再次尝试配置tftp server。在开发板uboot下使用tftp 命令下载时还是不能通。:(
```


```
        关了firewall 以后，点击apply以后。好像能通，但是又遇到下述问题：
		[u-boot@SMDK2440A]# tftp 0x30008000 uImage
		dm9000 i/o: 0x20000300, id: 0x90000a46 
		DM9000: running in 16 bit mode
		MAC: 08:08:11:18:12:27
		operating at 100M full duplex mode
		Using dm9000 device
		TFTP from server 192.168.1.111; our IP address is 192.168.1.226
		Filename 'uImage'.
		Load address: 0x30008000
		Loading: T 
		TFTP error: 'Permission denied' (0)
		Starting again


```


```
	找了半天，chmod -R /tftpboot， SElinux关闭，设置目录为 / 而不是 /tftpboot ，都不行。
	继续探索，发现SELinux没有关闭完全。于是执行以下操作：
		Fedora UI界面【System - Administration - SELinux Management 】在 SELinux Administration窗口中，Status选项下，设置:
		System Default Enforcing Mode: Disabled
	Current Enforcing Mode: Permissive
	测试下载uImage,成功。
```


```
附图：SELinux Administration窗口
![](https://img-blog.csdn.net/20140527221633000?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```


```


```


```


```





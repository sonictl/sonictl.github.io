---
layout: post
title:  "ROS进阶学习手记 10 - 用iRobot Create 2搭建自己的TurtleBot（1）- Introduction"
date: 2015-11-18 16:22:43 +0800
tags: 
slug: p20151118162243
---

# ROS进阶学习手记 10 - 用iRobot Create 2搭建自己的TurtleBot（1）- Introduction





## ROS进阶学习手记 10 - 搭建自己的TurtleBot（1）- Introduction


    时隔一两个月，再次回来继续写自己的ROS学习及应用手记。这一两个月里，我并没有闲着，而是照着《ROS\_by\_Example\_Hydro\_Vol1》基本走完了vol1全书所有的例程。但期间所遇到的一些问题，我并没有搞得很清楚。所以回到开始搭建TurtleBot的步骤，来重新梳理如何从攒一个硬件TurtleBot， 到可以执行 [SLAM](https://so.csdn.net/so/search?q=SLAM&spm=1001.2101.3001.7020) 任务的全过程。  
 


### 首先梳理一下手里有的材料：


* iRobot Create 2 移动机器人驱动平台，  [官方链接及资料](http://www.irobot.com/About-iRobot/STEM/Create-2.aspx)
* Mirko Ferrati's irobotCreate2\_ROS driver: <https://github.com/MirkoFerrati/irobotcreate2ros>
* 基于蓝牙无线通信协议的串口透明传输模块，[淘宝链接](https://item.taobao.com/item.htm?id=520975422997)（及配置用 USB-TTL电平 - RS232协议 串口转换模块）
* 机器人TurtleBot 控制用 Mini PC，装 ROS Hydro
* 大容量聚合物锂电池：12V 5.5AH, 用于机器人自带Mini PC的供电
* 华硕 Xtion Pro Live 三维摄像头
* 另一台Mini PC及显示器、键鼠
* 无线路由器，用于组建ROS多机局域网


### 系统组成：


![](https://img-blog.csdn.net/20151118160037098?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


### 实物照片：


 ![](https://img-blog.csdn.net/20151118160803700?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


### 要略过的一些内容：


参考《ROS by Example \_ Hydro \_ Vol1》书中所述，实现到“**7.5  Sending Twist Messages to a Real Robot**”  
 


中间的过程就按书中来进行即可。涉及到的比如设备驱动、ROS网络配置 等内容可参考本博客前的相关文章。


### ROS网络配置参数：




```
=--------==----------=---=----
     Log of Configurations:
=--------==----------=---=----

Physical machines:
  PC1 - Black Mini Computer (WLAN Mac: 74:44:01:67:xx:xx)
  PC2 - White Mini Computer (WLAN Mac: a0:21:b7:4e:xx:xx)

Hostname and Hosts file:
  PC1 - my_desktop
  PC2 - my_robot1

The host of ROS (the PC run roscore)
  PC2 - my_robot1

IP Address:
  PC1 - my_desktop - 192.168.1.103
  PC2 - my_robot1  - 192.168.1.102

The Env Vars:
  PC1 	-> $ROS_MASTER_URI = http://my_robot1:11311
	-> $ROS_HOSTNAME = my_desktop

  PC2 	-> $ROS_MASTER_URI = http://my_robot1:11311
	-> $ROS_HOSTNAME = my_robot1

Add the following lines at the end of ~/ .bashrc file:
  PC1: 
	
export ROS_HOSTNAME=my_desktop
	
export ROS_MASTER_URI=http://my_robot1:11311


  PC2:
	
export ROS_HOSTNAME=my_robot1
	
export ROS_MASTER_URI=http://my_robot1:11311



SSH Server:
  PC2:  sudo apt-get install openssh-server
  ![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

### 要在这里讨论的问题：



     终于进入正题，和《ROS by Example \_ Hydro \_ Vol1》(rbx)书中不同的是，这是仿 TurbleBot 的机器人，自己搭建的，所以难免有不兼容问题。


     1. 从《ROS by Example \_ Hydro \_ Vol1》rbx 书中看来，Turblebot应该是带了一个Gryro Scope 的加速度传感器，并且对它进行了校准，但实际Create2 是通过码盘进行distance的sense.  
          在 **roslaunch rbx1\_bringup turtlebot\_minimal\_create.launch** 这个命令运行时，PC（my\_robot1）会通过串口发送所有控制 Create2® 的命令。  
          这里调用的顺序是什么？启动了哪些nodes? 参数是如何设置的？  
 


         我们将在下一节讨论：  ROS进阶学习手记 10 - 搭建自己的TurtleBot（2）- 底盘驱动程序, 一个可选的驱动：[create\_autonomy](https://github.com/autonomylab/create_autonomy) driver package  
   
 


  
 


  
 


  
 





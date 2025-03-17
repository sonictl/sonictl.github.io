---
layout: post
title:  "在 Ubuntu 用 OpenNI支持 使用 ASUS Xtion 传感器"
date: 2015-11-06 15:18:39 +0800
tags: 
slug: p20151106151839
---

# 在 Ubuntu 用 OpenNI支持 使用 ASUS Xtion 传感器





## 在 Ubuntu 用 OpenNI支持 使用 ASUS Xtion Pro Live传感器  Installing the driver for Asus\_Xtion\_Pro\_Live in Ubuntu 12.04


### Pre-Reading:


    我们要用ROS来做机器人，同时要用 Asus\_Xtion\_Pro\_Live 这个传感器来获取深度信息和图像信息。首先要安装 Asus\_Xtion\_Pro\_Live 在 [Ubuntu](https://so.csdn.net/so/search?q=Ubuntu&spm=1001.2101.3001.7020) 系统中的驱动。


### Procedures:


Search online for the .iso disk file carrying the driver for Asus\_Xtion\_Pro\_Live. Like: "[V1049\_0430](http://www.thegamingcomputer.com/asus-xtion-pro-live-all-softwares-and-driver-for-win78vistaxp/)"


  
 ASUS Xtion Pro CD Readme file  
   
  OpenNI Framework v1.5.2.23  
  Sensor DDK v5.1.0.41  
  NITE v1.5.2.21  
  USB driver v3.1.3.1  
   
 \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*  
 Required software:  
   
 for WinXP:  
     Microsoft .NET 3.5: http://www.microsoft.com/en-us/download/details.aspx?id=21  
   
   
 


#### for Linux Ubuntu 10.10:


#### The installation steps as follows,


#### 1. install OpenNI package


#### 2. install Sensor package


#### 3. install NITE package (Necessary)


Steps for Install and Uninstall in Ubuntu Linux:


Install:  
   Unpack >> cd into folder >> $ sudo ./install.sh  
 


Uninstall:  
   $ sudo ./install.sh -u  
 


  
 


ps: Please download and install "OpenGL Utility Toolkit" package when you can not execute "NiViewer.exe".


### Testing and Using - 参照 《ROS by Example》来使用和调试


    然后就可以参照 《Ros by Example》来使用和调试。  
     \*\* Start your Asus\_Xtion\_Pro\_Live with ROS and rbx package:  
 



```
          $ roslaunch rbx1_vision openni_node.launch
```


    \*\* Test your Image in ROS:  
 



```
          $ rosrun image_view image_view image:=/camera/rgb/image_color
```

    \*\* You can also test the depth image from your camera using the ROS  **disparity\_view** node:  
 



```
          $ rosrun image_view disparity_view image:=/camera/depth/disparity
```

  
 


  




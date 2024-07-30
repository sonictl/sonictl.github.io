---
layout: post
title:  "ROS：使用ROS_arduino_bridge Stack"
date: 2016-10-24 21:43:12 +0800
tags: 
slug: p20161024214312
---

# ROS：使用ROS_arduino_bridge Stack





##### 使用ROS\_arduino\_bridge Stack







*使ROS得到odom信息* 和 *使move base(移动底盘)获得 cmd\_vel信息*，需要个桥梁。


而修改 ROS\_arduino\_bridge Stack 后可以完成此任务。   
 


  
 目的就是 发布 odom 和 使movebase（移动底盘）得到cmd\_vel 信息，从而控制底盘运动。 
   
 斟酌后，Finally，使用stack(后来叫做 
 [Metapackage](http://wiki.ros.org/Metapackages)): ROS Arduino Bridge 来完成此项工作： 
 ref:


  <http://wiki.ros.org/ros_arduino_bridge>


  <https://github.com/hbrobotics/ros_arduino_bridge/blob/indigo-devel/README.md>


### 使用ROS\_arduino\_bridge Stack


  参照 ref 中github中 readme.md 进行配置。


  最后一个Section ： Note中说明了如果在硬件与它们不匹配的情况下依然使用这个 stack 帮助快速建立 ROS and [arduino](https://so.csdn.net/so/search?q=arduino&spm=1001.2101.3001.7020)连接的方法。


  check out 代码命令：



```
$ cd ~/catkin_ws/src
$ git clone https://github.com/hbrobotics/ros_arduino_bridge.git
$ cd rbx1
$ git checkout hydro-devel
$ cd ~/catkin_ws
$ catkin_make
$ source ~/catkin_ws/devel/setup.bash

```

    
 


### install the ROSArduinoBridge library for Arduino


ref : <https://www.arduino.cc/en/Guide/Libraries#toc5>


来手动滴add Library:   
 


Restart the Arduino application. Make sure the new library appears in the  **Sketch->Include Library** menu item of the software. That's it! You've installed a library!  
 


  
 


  
 









---
layout: post
title:  "ROS进二阶学习笔记（9）-- 关于Overlay：重名 package 在不同catkin workspace 中，"
date: 2017-01-13 17:08:03 +0800
tags: 
slug: p20170113170803
---

# ROS进二阶学习笔记（9）-- 关于Overlay：重名 package 在不同catkin workspace 中，





  
 


要把ROS玩转，必须把 catkin 玩转。


http://wiki.ros.org/catkin/Tutorials


其中，Overlay问题是 重名 [package](https://so.csdn.net/so/search?q=package&spm=1001.2101.3001.7020) 在不同catkin workspace 中时，如何处理他们的关系。


一个检查的命令： `echo $ROS_PACKAGE_PATH` 可检查 overlay


  
 


用来设置path的命令们：  
 


$ source /opt/ros/indigo/setup.bash  
 $ source ~/catkin\_ws/devel/setup.bash  
 $ source ~/catkin\_ws2/devel/setup.bash


  
 可得到：  
 $ echo $ROS\_PACKAGE\_PATH  
 >> /root/catkin\_ws2/src:/root/catkin\_ws/src:/opt/ros/indigo/share:/opt/ros/indigo/stacks  
 


========== 正片开始： ==========



## Overlaying with catkin workspaces

 ref: 
<http://wiki.ros.org/catkin/Tutorials/workspace_overlaying>

[Overview:]  
 


  
 


  
 





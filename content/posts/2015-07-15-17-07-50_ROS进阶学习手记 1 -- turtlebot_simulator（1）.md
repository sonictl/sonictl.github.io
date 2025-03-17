---
layout: post
title:  "ROS进阶学习手记 1 -- turtlebot_simulator（1）"
date: 2015-07-15 17:07:50 +0800
tags: 
slug: p20150715170750
---

# ROS进阶学习手记 1 -- turtlebot_simulator（1）





  
 


  
 


这是ROS入门手记的最后一节，指明了ROS进阶学习的方向：


       http://blog.csdn.net/sonictl/article/details/46893443


### ROS进阶学习手记 1 -- turtlebot\_simulator（1）


#### 1. 用turtlebot来进行机器人模拟


    Robot Stack下的TurtleBot [Package](https://so.csdn.net/so/search?q=Package&spm=1001.2101.3001.7020),  link: <http://wiki.ros.org/Robots/TurtleBot>


    由于我们只要了解他的simulator, 故turtlebot\_simulator Stack的link：   [wiki.ros.org/turtlebot\_simulator](http://wiki.ros.org/turtlebot_simulator)  
 


    这个模拟的空间叫做Gazebo World, 故它的link: <http://wiki.ros.org/turtlebot_simulator/Tutorials/hydro/Explore%20the%20Gazebo%20world>



ROS中提供了很多强大的功能，我们学习完前面的基本知识之后要继续进行深入。


  

    turtlebot\_simulator Stack的link：  [wiki.ros.org/turtlebot\_simulator](http://wiki.ros.org/turtlebot_simulator)  
 


   我们跑了两个命令，把simulator打开看看了：



       Install the software  




```
         $ sudo apt-get install ros-hydro-turtlebot-simulator
```



       Start the simulation  




```
         $ source /opt/ros/hydro/setup.bash
         $ roslaunch turtlebot_gazebo turtlebot_empty_world.launch
```

   得到下图：  
 


![](https://img-blog.csdn.net/20150715174654881?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


当然你可能得到一个 30 秒无法连接Master 的问题，最后找了一圈，也不知怎么就解决了~~  
 如果你遇到，可能这里有点帮助：http://answers.ros.org/question/65402/gazebo-fail-to-connect-to-master-in-30-seconds-it-does-not-run-from-the-very-first-time/  
 



#### gazebo



　　这个工具是ROS中的物理仿真环境，gazebo本身就是一款机器人的仿真软件，基于ODE的物理引擎，可以模拟机器人以及环境中的很多物理特性，这个软件可以稍作了解，并不是后面开发所必须要的。


　　教程见：  http://wiki.ros.org/simulator\_gazebo/Tutorials/StartingGazebo


  
 


  
 



#### 2. 接下来要学习的工具：


#### **1、rviz**



　　rviz是ROS中一款强大的3D可视化工具，这个玩意在后面可是要频繁用到的，是必须要弄明白的，详细的教程可以参考wiki：http://wiki.ros.org/rviz/UserGuide


  
    　我们可以在里面创建自己的机器人，并且让机器人动起来。还可以创建地图，显示3D点云等等，总之，想在ROS中显示的东东都可以在这里显示出来。当然这些显示都是通过消息的订阅来完成的，机器人通过ROS发布数据，rviz订阅消息接收数据，然后显示，这些数据也是有一定的数据格式的，可以参考下面的链接：


　　http://www.ros.org/wiki/rviz/DisplayTypes(待改)


　　看到上面的机器人了吧，是不是很酷，在rviz中，这样的机器人模型是通过urdf文件描述的，具体urdf文件怎么写，参考wiki：


　　http://www.ros.org/wiki/urdf（待改）


#### 


#### **2、tf**


　　tf是ROS中的坐标变换系统，在机器人的建模仿真中经常用到。


　　ROS中主要有两种坐标系：


　　(1)固定坐标系：用于表示世界的参考坐标系;


　　(2)目标坐标系：相对于摄像机视角的参考坐标系。


　　教程见：http://www.ros.org/wiki/tf（待改）


  

#### 


  




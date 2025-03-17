---
layout: post
title:  "ROS_Navigation专题1 - Introduction and Learning"
date: 2016-04-14 11:06:14 +0800
tags: 
slug: p20160414110614
---

# ROS_Navigation专题1 - Introduction and Learning





## ROS\_Navigation专题1 - Introduction and Learning Resources


  Before going further, it is highly recommended that the reader check out the Navigation[Robot Setup](http://wiki.ros.org/navigation/Tutorials/RobotSetup) tutorial on the ROS Wiki.  
   This tutorial provides an excellent overview of the ROS navigation stack.  
   For an even better understanding, check out all of the  [Navigation Tutorials](http://wiki.ros.org/navigation/Tutorials).  
   And for a superb introduction to the mathematics underlying SLAM, check out Sebastian Thrun's online[Artificial Intelligence course](http://write.blog.csdn.net/postedit/www.udacity.com/overview/Course/cs373/CourseRev/apr2012) on Udacity.


  If you own a TurtleBot, you might want to skip directly to the  [TurtleBot SLAM tutorial](http://wiki.ros.org/turtlebot_navigation/Tutorials/Build%20a%20map%20with%20SLAM) on the ROS Wiki.  
   Another affordable SLAM robot is the Neato XV-11 vacuum cleaner which includes a 360-degree laser scanner. In fact, you can run the complete Navigation Stack using the XV-11 thanks to the neato\_robot ROS stack by Michael Ferguson.  
   
 In this special subject, we will cover the three essential ROS packages that make up the core of the Navigation Stack:  
 


* •  [move\_base](http://write.blog.csdn.net/postedit/wiki.ros.org/move_base) for moving the robot to a goal pose within a given reference frame
* •  [gmapping](http://write.blog.csdn.net/postedit/wiki.ros.org/gmapping) for creating a map from laser scan data (or simulated laser data from a depth camera)
* •  [amcl](http://write.blog.csdn.net/postedit/wiki.ros.org/amcl) for localization using an existing map



 ---- 


 In this case, I fell that starting from ROS  
 [Navigation Tutorials](http://wiki.ros.org/navigation/Tutorials) could be a better way to learn. 
   
 ---- 
   
 So I will follow the outline below: 
   

#### 1. Navigation is an ROS stack named as "Navigation"

    It contains packages listed as below: 
   
    amcl\* 
   
    base\_local\_planner 
   
    carrot\_planner 
   
    clear\_costmap\_recovery 
   
    costmap\_2d 
   
    dwa\_local\_planner 
   
    fake\_localization 
   
    global\_planner 
   
    map\_server\* 
   
    move\_base\* 
   
    move\_base\_msgs 
   
    move\_slow\_and\_clear 
   
    nav\_core 
   
    navfn 
   
    robot\_pose\_ekf 
   
    rotate\_recovery 
   
    voxel\_grid 
   

  

#### 2. First, follow the tutorials in this order:

    1) The introduction to Navigation: 
 <http://wiki.ros.org/navigation?distro=hydro#Tutorials>
  
    2) The specific tutorial webpage: 
 <http://wiki.ros.org/navigation/Tutorials>
  

  
    
 Note:
  

##### 使用导航功能包集(Navigation Stacks)的先决条件是：

        >机器人必须运行ROS， 
   
        >有一个tf变换树， 
   
        >使用正确的ROS Message types发布传感器数据。 
   
    而且，我们需要在高层为一个具有一定形状和动力学特点的机器人配置Navigation Stacks。本手册指导配置一个典型的Navigation Stacks。 
   
    
   

##### 硬件限制：

        虽然Navigation Stacks被设计成尽可能的通用，在使用时仍然有三个主要的硬件限制： 
   

* 它是为差分驱动的完全约束的轮式机器人设计的。它假设移动基站受到理想的运动命令的控制并可实现预期的结果，命令的格式为：x速度分量，y速度分量，角速度(theta)分量。
* 它需要在移动基站上安装一个平面二维激光。这个激光用于构建地图和定位。
* Navigation Stacks是为正方形的机器人开发的，所以方形或圆形的机器人将是性能最好的。 它也可以工作在任意形状和大小的机器人上，但是较大的矩形机器人将很难通过狭窄的空间，例如门道。

    
   

#### 3. Second, follow the  [tutorial](http://wiki.ros.org/navigation/Tutorials) webpage:

    --------------------->>>> 
 ##### Configuring and Using the Navigation Stack




1. [Setting up your robot using tf](http://wiki.ros.org/navigation/Tutorials/RobotSetup/TF)  This tutorial provides a guide to set up your robot to start using tf.
2. [No Title](http://wiki.ros.org/evarobot_navigation/Tutorials/indigo) No Description
3. [Navigation of the Evarobot in Gazebo](http://wiki.ros.org/evarobot_navigation/Tutorials/indigo/Navigation%20of%20the%20Evarobot%20in%20Gazebo)  How to navigate evarobot in Gazebo with a previously known map.
4. [Basic Navigation Tuning Guide](http://wiki.ros.org/navigation/Tutorials/Navigation%20Tuning%20Guide)  This guide seeks to give some standard advice on how to tune the[ROS Navigation Stack](http://wiki.ros.org/navigation) on a robot. This guide is in no way comprehensive, but should give some insight into the process. I'd also encourage folks to make sure they've read the[ROS Navigation Tutorial](http://wiki.ros.org/navigation/Tutorials/RobotSetup) before this post as it gives a good overview on setting the navigation stack up on a robot wheras this guide just gives advice on the process.
5. [Bilinen Bir Haritada Otonom Evarobot Navigasyonu](http://wiki.ros.org/evarobot_navigation/Tutorials/indigo/Otonom%20Evarobot%20Navigasyonu)  Daha önceden çıkartılmış haritada otonom robot navigasyonu.
6. [Using rviz with the Navigation Stack](http://wiki.ros.org/navigation/Tutorials/Using%20rviz%20with%20the%20Navigation%20Stack)  This tutorial provides a guide to using rviz with the navigation stack to initialize the localization system, send goals to the robot, and view the many visualizations that the navigation stack publishes over ROS.
7. [Installing](http://wiki.ros.org/mrpt_navigation/Tutorials/Installing)  Instructions to install and compile this package
8. [Navigation with Robotino](http://wiki.ros.org/robotino_navigation/Tutorials/Navigation%20with%20Robotino)  Autonomous navigation of a known map with Robotino
9. [Autonomous Navigation of a Known Map with Evarobot](http://wiki.ros.org/evarobot_navigation/Tutorials/indigo/Autonomous%20Navigation%20of%20a%20Known%20Map%20with%20Evarobot)  How to navigate autonomously the Evarobot with known map.
10. [Setup and Configuration of the Navigation Stack on a Robot](http://wiki.ros.org/navigation/Tutorials/RobotSetup)  This tutorial provides step-by-step instructions for how to get the navigation stack running on a robot. Topics covered include: sending transforms using tf, publishing odometry information, publishing sensor data from a laser over ROS, and basic navigation stack configuration.
11. [Publishing Odometry Information over ROS](http://wiki.ros.org/navigation/Tutorials/RobotSetup/Odom)  This tutorial provides an example of publishing odometry information for the navigation stack. It covers both publishing the nav\_msgs/Odometry message over ROS, and a transform from a "odom" coordinate frame to a "base\_link" coordinate frame over tf.
12. [Mapping with Robotino](http://wiki.ros.org/robotino_navigation/Tutorials/Mapping%20with%20Robotino)  Explains how to build a map with Robotino using gmapping
13. [Publishing Sensor Streams Over ROS](http://wiki.ros.org/navigation/Tutorials/RobotSetup/Sensors)  This tutorial provides examples of sending two types of sensor streams,sensor\_msgs/LaserScan messages and sensor\_msgs/PointCloud messages over ROS.
14. [Sending Goals to the Navigation Stack](http://wiki.ros.org/navigation/Tutorials/SendingSimpleGoals)  The Navigation Stack serves to drive a mobile base from one location to another while safely avoiding obstacles. Often, the robot is tasked to move to a goal location using a pre-existing tool such as rviz in conjunction with a map. For example, to tell the robot to go to a particular office, a user could click on the location of the office in a map and the robot would attempt to go there. However, it is also important to be able to send the robot goals to move to a particular location using code, much like rviz does under the hood. For example, code to plug the robot in might first detect the outlet, then tell the robot to drive to a location a foot away from the wall, and then attempt to insert the plug into the outlet using the arm. The goal of this tutorial is to provide an example of sending the navigation stack a simple goal from user code.
15. [Gazebo'da Evarobot Navigasyonu](http://wiki.ros.org/evarobot_navigation/Tutorials/indigo/Gazebo%20Evarobot%20Navigasyonu)  Çıkartılmış harita üzerinden Gazebo'da otonom Evarobot navigasyonu.





##### Configuring and Using the Global Planner of the Navigation Stack



1. [Writing A Global Path Planner As Plugin in ROS](http://wiki.ros.org/navigation/Tutorials/Writing%20A%20Global%20Path%20Planner%20As%20Plugin%20in%20ROS): This tutorial presents the steps for writing and using a global path planner in ROS as a plugin.



##### Navigation stack and stage



To use navigation stack with stage, check the  [navigation\_stage](http://wiki.ros.org/navigation_stage) package. 



##### Robot Specific Configurations



This section contains information on configuring particular robots with the navigation stack. Please help us by adding information on your robots.



#### Erratic



The  [erratic\_navigation](http://wiki.ros.org/erratic_navigation) package contains configuration and launch files for running the navigation stack on Erratic robot. The[erratic\_navigation\_apps](http://wiki.ros.org/erratic_navigation_apps) package contains example launch files that will start the navigation stack in three different configurations:


* navigation with existing static map
* navigation with SLAM for building a map of the area
* navigation in local odometric frame without a map and localization:


The  [erratic\_teleop](http://wiki.ros.org/erratic_teleop) package contains a keyboard teleoperation node for driving the robot (e.g. while in SLAM mode).



#### Pioneer



**TODO** 



#### iRobot Create



**TODO**
  

#### 4.


  


  
 





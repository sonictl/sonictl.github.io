---
layout: post
title: 'ROS进阶学习笔记（10）- 搭建自己的Turtlebot(5) - Interactive Makers'
date: 2015-12-23 09:10:00 +0800
category: from_cnblogs
slug: p20151223091000
---


<h1>&nbsp; 用interactive_makers控制Turtlebot移动</h1>
<br>
interactive_makers 是Willow Garage公司开发的一个虚拟控制工具，可通过鼠标在虚拟环境中的操作，完成实际机器人运动的拖动运动控制。<br>
<br>
<h2>参考：</h2>
&nbsp;&nbsp; ref1：<a target="_blank" target="_blank" href="http://wiki.ros.org/turtlebot_interactive_markers/Tutorials/UsingTurtlebotInteractiveMarkers"><br>
</a>&nbsp;&nbsp; <a target="_blank" target="_blank" href="http://wiki.ros.org/turtlebot_interactive_markers/Tutorials/UsingTurtlebotInteractiveMarkers">
http://wiki.ros.org/turtlebot_interactive_markers/Tutorials/UsingTurtlebotInteractiveMarkers</a><br>
（groovy version）<br>
<br>
&nbsp;&nbsp; === rvizTutorialsInteractive Markers: Getting Started ===<br>
&nbsp;&nbsp; ref2：<br>
&nbsp;&nbsp; <a target="_blank" target="_blank" href="http://wiki.ros.org/rviz/Tutorials/Interactive%20Markers%3A%20Getting%20Started">
http://wiki.ros.org/rviz/Tutorials/Interactive%20Markers%3A%20Getting%20Started</a><br>
<br>
就应该实现ref1页面中，视频里的功能。<br>
<br>
<h2>具体步骤：</h2>
在my_robot1的Terminal里运行：<br>
<pre>&nbsp;$ roslaunch rbx1_bringup turtlebot_minimal_create.launch</pre>
<br>
在my_Desktop的Terminal里运行：<br>
<pre>&nbsp;$ roslaunch turtlebot_interactive_markers interactive_markers.launch
 $ roslaunch turtlebot_rviz_launchers view_robot.launch</pre>
<br>
前提是按ref1安装了turtlebot_interactive_markers:<br>
<pre>&nbsp;$ rosmake turtlebot_interactive_markers</pre>
<br>
   

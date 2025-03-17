---
layout: post
title:  "Using Create2(Roomba 620) as your turtlebot driving base."
date: 2015-12-07 16:55:36 +0800
tags: 
slug: p20151207165536
---

# Using Create2(Roomba 620) as your turtlebot driving base.





This message has been posted on  [goolge group](https://groups.google.com/forum/#!topic/irobot-create-forum/rZ3J9IAGDGo): <https://groups.google.com/forum/#!topic/irobot-create-forum/rZ3J9IAGDGo>  
 You can check out the newest posts there!


## Problem/Solution: Using Create2(Roomba 620) as your turtlebot driving base.


使用 iRobot Create 2 作为 Turtlebot 的驱动底盘  see this blog: [ROS 进阶学习笔记（14） - About driving your iRobot Roomba/Create](http://blog.csdn.net/sonictl/article/details/51248683)   
 


### [Problem]：



  
 I met the incompatibility problem when I was trying to build and test my own\_turtlebot which is using iRobot Create 2(Roomba 620). When I use the file "turtlebot\_minimal\_create. 
 launch" offered by rbx('ROS by Example\_Hydro\_vol1') book. When I was doing the linear and angular calibration as the book described, I meet unexpectable driving actions made by my create2. 
   

  
 As the writor of rbx\_hydro\_vol1 book was using original Turtlebot which is using iRobot Create as the driving base, however, iRobot Create is out of sale and has been replaced by iRobot\_Create\_2 as I know. 
   

  
 iRobot co. seems not to offer iRobot\_Create, so We can only buy iRobot\_Create\_2 to build our non-original-turtlebot. Thus, we can not use the launch file offered by rbx book directly. 
   

  

### [Solution]

 Looking into the file "turtlebot\_minimal\_create. 
 launch", we can find it refered a file "tb\_create\_mobile\_base.launch. 
 xml". This file defined parameters about driving our base. 
   

  
 Add or modify the parameter definition lines as below: 
   




   
<param

name
=
"has\_gyro"

value
=
"false"

/>
  
<!--Create2 has no gyro sensor-->
  
    
<param
 
name
=
"turtlebot\_node/odom\_angular\_scale\_correction"

value
=
"1.0"
/>
  
    
<param
 
name
=
"turtlebot\_node/odom\_linear\_scale\_correction"

value
=
"1.0"
/>
  
    
<param
 
name
=
"robot\_type"

value
=
"roomba"
/>

<!--"create", or "roomba" for Create2-->


  
 Here, param " 
 robot\_type" will make the file "robot\_types.py" know which name,baudrate,sensor\_handler, 
 wheel\_separation to choose. You can ran command below to find out the file"robot\_types.py" in you Ubuntu+ROS system: 
   

   $ cd / && sudo find -name "robot\_types.py"
  

  
 So, on my understanding of the rbx book, the driving base (Create2) will only use ros topic 
  /odomas the driving/navigation information. If you added gyro\_sensor(like ADXR652) into this driving system(based on Create2), please share your solutions/steps with us. I think the writer and me will appreciate that! 
   

  

### [Contact]

 If you meet any new thoughts/ideas/solutions/ 
 problems about this topic, welcome to join here and discuss about it. Thanks!! 

  





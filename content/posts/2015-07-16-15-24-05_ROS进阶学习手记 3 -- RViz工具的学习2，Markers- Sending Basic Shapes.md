---
layout: post
title:  "ROS进阶学习手记 3 -- RViz工具的学习2，Markers- Sending Basic Shapes"
date: 2015-07-16 15:24:05 +0800
tags: 
slug: p20150716152405
---

# ROS进阶学习手记 3 -- RViz工具的学习2，Markers- Sending Basic Shapes






### Markers: Sending Basic Shapes (C++)

 --------------------------- 
  


       我是从这里开始：http://wiki.ros.org/rviz  
            在它之下，先玩完这一部分：http://wiki.ros.org/rviz/UserGuide 


           今天要玩这一部分： http://wiki.ros.org/rviz/Tutorials/Markers%3A%20Basic%20Shapes


---------------------------


**Description:** Shows how to use  [visualization\_msgs/Marker](http://docs.ros.org/api/visualization_msgs/html/msg/Marker.html) messages to send basic shapes (cube, sphere, cylinder, arrow) to rviz.



### 2. Intro




Unlike other displays, the  [Marker Display](http://wiki.ros.org/rviz/DisplayTypes/Marker) lets you visualize data in rviz without rviz knowing anything about interpreting that data. Instead, primitive objects are sent to the display through[visualization\_msgs/Marker](http://docs.ros.org/api/visualization_msgs/html/msg/Marker.html) messages, which let you show things like arrows, boxes, spheres and lines.

 This tutorial will show you how to send the four basic shapes (boxes, spheres, cylinders, and arrows). We'll create a program that sends out a new marker every second, replacing the last one with a different shape. 

### 3. Create a Scratch Package




Before getting started, let's create a scratch package to calledusing\_markers, somewhere**in your package path**:(exbot@ubuntu:~/catkin\_ws/src)







```
catkin_create_pkg using_markers roscpp visualization_msgs
```


  


### 4. Sending Markers





#### The Code



Paste the following into src/basic\_shapes.cpp:



*<https://raw.github.com/ros-visualization/visualization_tutorials/indigo-devel/visualization_marker_tutorials/src/basic_shapes.cpp>*
  



```
1. #include <ros/ros.h>
2. #include <visualization_msgs/Marker.h>
3. 
4. int main( int argc, char** argv )
5. {
6. ros::init(argc, argv, "basic_shapes");
7. ros::NodeHandle n;
8. ros::Rate r(1);
9. ros::Publisher marker_pub = n.advertise<visualization_msgs::Marker>("visualization_marker", 1);
10. 
11. // Set our initial shape type to be a cube
12. uint32_t shape = visualization_msgs::Marker::CUBE;
13. 
14. while (ros::ok())
15. {
16. visualization_msgs::Marker marker;
17. // Set the frame ID and timestamp.  See the TF tutorials for information on these.
18. marker.header.frame_id = "/my_frame";
19. marker.header.stamp = ros::Time::now();
20. 
21. // Set the namespace and id for this marker.  This serves to create a unique ID
22. // Any marker sent with the same namespace and id will overwrite the old one
23. marker.ns = "basic_shapes";
24. marker.id = 0;
25. 
26. // Set the marker type.  Initially this is CUBE, and cycles between that and SPHERE, ARROW, and CYLINDER
27. marker.type = shape;
28. 
29. // Set the marker action.  Options are ADD, DELETE, and new in ROS Indigo: 3 (DELETEALL)
30. marker.action = visualization_msgs::Marker::ADD;
31. 
32. // Set the pose of the marker.  This is a full 6DOF pose relative to the frame/time specified in the header
33. marker.pose.position.x = 0;
34. marker.pose.position.y = 0;
35. marker.pose.position.z = 0;
36. marker.pose.orientation.x = 0.0;
37. marker.pose.orientation.y = 0.0;
38. marker.pose.orientation.z = 0.0;
39. marker.pose.orientation.w = 1.0;
40. 
41. // Set the scale of the marker -- 1x1x1 here means 1m on a side
42. marker.scale.x = 1.0;
43. marker.scale.y = 1.0;
44. marker.scale.z = 1.0;
45. 
46. // Set the color -- be sure to set alpha to something non-zero!
47. marker.color.r = 0.0f;
48. marker.color.g = 1.0f;
49. marker.color.b = 0.0f;
50. marker.color.a = 1.0;
51. 
52. marker.lifetime = ros::Duration();
53. 
54. // Publish the marker
55. while (marker_pub.getNumSubscribers() < 1)
56. {
57. if (!ros::ok())
58. {
59. return 0;
60. }
61. ROS_WARN_ONCE("Please create a subscriber to the marker");
62. sleep(1);
63. }
64. marker_pub.publish(marker);
65. 
66. // Cycle between different shapes
67. switch (shape)
68. {
69. case visualization_msgs::Marker::CUBE:
70. shape = visualization_msgs::Marker::SPHERE;
71. break;
72. case visualization_msgs::Marker::SPHERE:
73. shape = visualization_msgs::Marker::ARROW;
74. break;
75. case visualization_msgs::Marker::ARROW:
76. shape = visualization_msgs::Marker::CYLINDER;
77. break;
78. case visualization_msgs::Marker::CYLINDER:
79. shape = visualization_msgs::Marker::CUBE;
80. break;
81. }
82. 
83. r.sleep();
84. }
85. }
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  



Now edit the CMakeLists.txt file in yourusing\_markers package, and add:




```
add_executable(basic_shapes src/basic_shapes.cpp)
target_link_libraries(basic_shapes ${catkin_LIBRARIES})
```


to the bottom. 
具体的Code Explaining见本文的来源网页：http://wiki.ros.org/rviz/Tutorials/Markers%3A%20Basic%20Shapes  
 


  
 



#### 4.3 Building the Code




You should be able to build the code with:  







```
$ cd %TOP_DIR_YOUR_CATKIN_WORKSPACE%
$ catkin_make

```

   实际地址：exbot@ubuntu:~/catkin\_ws$ catkin\_make



#### 4.4 Running the Code



You should be able to run the code with:  




```
rosrun using_markers basic_shapes
```



### 5. Viewing the Markers



Now that you're publishing markers, you need to set rviz up to view them. First, make sure rviz is built:




```
rosmake rviz
```


Now, run rviz: 




```
rosrun using_markers basic_shapes
rosrun rviz rviz
```


If you've never used rviz before, please see the  [User's Guide](http://wiki.ros.org/rviz/UserGuide) to get you started. 





The first thing to do, because we don't have any  [tf](http://wiki.ros.org/tf) transforms setup, is to set the  [Fixed Frames](http://wiki.ros.org/rviz/UserGuide#Frames) to the frame we set the marker to above, /my\_frame. In order to do so, set the Fixed Frame field to "/my\_frame".










Next add a  [Markers](http://wiki.ros.org/rviz/DisplayTypes/Marker) display. Notice that the default topic specified,  visualization\_marker, is the same as the one being published.  

 You should now see a marker at the origin that changes shape every second: 
  

![](https://img-blog.csdn.net/20150717134719877?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


  
 


### 6. More Information


For more information on the different types of markers beyond the ones shown here, please see the 
[Markers Display Page](http://wiki.ros.org/rviz/DisplayTypes/Marker)
  


  
 


  
 


然后我还突发奇想，复习一下前面初级篇的内容，来完成一个任务：


任务1：   
     利用 turtlesim turtlesim\_node，完成一个从键盘输入int64 x, int64 y, int64 angle, int64\_velocity的运动。  
     分析： 先用my\_drivenode1向topic=/turtle1/cmd\_vel 发送Message  
                 再接受键盘输入，从而模拟teleop\_key 的工作。  
 


                   
       
   
   
 


  
 


  
 


  
 





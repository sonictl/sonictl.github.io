---
layout: post
title:  "ROS进阶学习手记 10 - 搭建自己的TurtleBot（3）- Velocity Calibration"
date: 2015-11-20 18:07:00 +0800
tags: 
slug: p20151120180700
---

# ROS进阶学习手记 10 - 搭建自己的TurtleBot（3）- Velocity Calibration





## ROS进阶学习手记 10 - 搭建自己的TurtleBot（3）- [Velocity](https://so.csdn.net/so/search?q=Velocity&spm=1001.2101.3001.7020) Calibration


    上一篇我们分析了 rbx 书籍中，对于底板驱动相关的.launch文件，turtlebot\_minimal\_create.launch， 现在要实验一下相关的配置以后，速度值是否与设定值一致。


    要用到的.launch 文件： turtlebot\_minimal\_create\_me.launch ; 使用 rostopic pub 命令发布驱动指令。


    运行命令行：


    先把rbx包source一下：  
 



```
    $ cd ~/catkin_ws
    $ catkin_make
    $ source ~/catkin_ws/devel/setup.bash
```

    再在子机上（my\_robot1）启动 底盘 驱动： (通过vi工具修改了子机上的tb\_create\_mobile\_base.launch.xml文件)  
 




```
  $ roslaunch rbx1_bringup turtlebot_minimal_create.launch
```


在校准之前，务必参考我博文：[Using Create2(Roomba 620) as your turtlebot driving base.](http://blog.csdn.net/sonictl/article/details/50207949) 


### 校准线速度：



    通过母机发指令：  
 



```
  $ rostopic pub -r 10 /cmd_vel geometry_msgs/Twist '{linear: {x: 0.1, y: 0, z: 0}, angular: {x: 0, y: 0, z: 0}}'
```
    按照rbx书上的说法：Linear velocities are specified in meters per second and angular velocities are given in radians per second. (1 radian equals approximately 57 
  
 degrees.) 单位是 m/s 和 rad/s. (1 
*rad* = 180/pi  
*degree*) 

    实测速度在 0.12 m/s 左右。实验距离：1米。耗时8.5s左右。  
 


### 校准角速度：


    通过母机发指令：  
 



```
  $ rostopic pub -r 10 /cmd_vel geometry_msgs/Twist '{linear: {x: 0, y: 0, z: 0}, angular: {x: 0, y: 0, z: 0.628}}'
```
    0.628 rad/s 是指令发送角速度，意味着转1圈 2\*3.14 rad = 6.28 rad , 耗时应：10秒， 实测耗时：7.55s, 
  
     so, 实际角速度 = 实际转动rad数/耗用时间 = 6.28/(7.55)s = 0.83 rad/s = 0.628\* 1.32 
  
     这里我准备做个表格，画画图，看看角速度校准应该怎么弄。（地面条件：磨石地板，粗糙度：0.3-0.5mm） 
  


    




| # | 转动圈数 | 转动角度（rad） | 设置角速度rad/s | 理论耗用时间s | 实际耗用时间s | 实际角速度rad/s | 修正系数 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 1 | 6.28 | 0.100 | 62.80 | 15.14 | 0.41 | 4.148 |
| 2 | 1 | 6.28 | 0.200 | 31.40 | 15.03 | 0.42 | 2.089 |
| 3 | 1 | 6.28 | 0.300 | 20.93 | 15.05 | 0.42 | 1.391 |
| 4 | 1 | 6.28 | 0.350 | 17.94 | 10.18 | 0.62 | 1.763 |
| 5 | 1 | 6.28 | 0.400 | 15.70 | 10.10 | 0.62 | 1.554 |
| 6 | 1 | 6.28 | 0.500 | 12.56 | 10.20 | 0.62 | 1.231 |
| 7 | 1 | 6.28 | 0.628 | 10.00 | 7.55 | 0.83 | 1.325 |
| 8 | 1 | 6.28 | 0.750 | 8.37 | 6.13 | 1.02 | 1.366 |
| 9 | 1 | 6.28 | 1.000 | 6.28 | 5.12 | 1.23 | 1.227 |
| 10 | 1 | 6.28 | 1.400 | 4.49 | 3.83 | 1.64 | 1.171 |
| 11 | 1 | 6.28 | 1.600 | 3.93 | 3.82 | 1.64 | 1.027 |
| 12 | 1 | 6.28 | 1.800 | 3.49 | 3.36 | 1.87 | 1.038 |
| 13 | 1 | 6.28 | 2.000 | 3.14 | 2.81 | 2.23 | 1.117 |

      
  


![](https://img-blog.csdn.net/20151120150824408?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


    参考线方程： y = x + 0.2


    **参考配置：**慢速档： 0.3-0.5； 线性档： 0.5 - 1.5 ； 高速档：2.0以上


### 如何根据线速度和角速度估计机器人的位置：


   ![](https://img-blog.csdn.net/20151120200228978?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


  
 


     
 


  
 


  
 


  
 


  
 





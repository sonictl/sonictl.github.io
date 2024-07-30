---
layout: post
title:  "ROS进阶学习手记 10 - 搭建自己的TurtleBot（4）- Nodes of Base Driver"
date: 2015-11-25 09:35:47 +0800
tags: 
slug: p20151125093547
---

# ROS进阶学习手记 10 - 搭建自己的TurtleBot（4）- Nodes of Base Driver





## ROS进阶学习手记 10 - 搭建自己的TurtleBot（4）- Nodes of Base Driver


Here we are going to continue playing with the launchfile:[ **turtlebot\_minimal\_create.launch** ]  
 


and look into the nodes, topics, messages, publisher, and subscriber about that.


Let's refer the log printed when we start this launchfile here:




```
SUMMARY  
========  
  
PARAMETERS  
 * /cmd_vel_mux/yaml_cfg_file  
 * /diagnostic_aggregator/analyzers/digital_io/path  
 * /diagnostic_aggregator/analyzers/digital_io/startswith  
 * /diagnostic_aggregator/analyzers/digital_io/timeout  
 * /diagnostic_aggregator/analyzers/digital_io/type  
 * /diagnostic_aggregator/analyzers/mode/path  
 * /diagnostic_aggregator/analyzers/mode/startswith  
 * /diagnostic_aggregator/analyzers/mode/timeout  
 * /diagnostic_aggregator/analyzers/mode/type  
 * /diagnostic_aggregator/analyzers/nodes/contains  
 * /diagnostic_aggregator/analyzers/nodes/path  
 * /diagnostic_aggregator/analyzers/nodes/timeout  
 * /diagnostic_aggregator/analyzers/nodes/type  
 * /diagnostic_aggregator/analyzers/power/path  
 * /diagnostic_aggregator/analyzers/power/startswith  
 * /diagnostic_aggregator/analyzers/power/timeout  
 * /diagnostic_aggregator/analyzers/power/type  
 * /diagnostic_aggregator/analyzers/sensors/path  
 * /diagnostic_aggregator/analyzers/sensors/startswith  
 * /diagnostic_aggregator/analyzers/sensors/timeout  
 * /diagnostic_aggregator/analyzers/sensors/type  
 * /diagnostic_aggregator/base_path  
 * /diagnostic_aggregator/pub_rate  
 * /robot/name  
 * /robot/type  
 * /robot_description  
 * /robot_pose_ekf/freq  
 * /robot_pose_ekf/imu_used  
 * /robot_pose_ekf/odom_used  
 * /robot_pose_ekf/output_frame  
 * /robot_pose_ekf/publish_tf  
 * /robot_pose_ekf/sensor_timeout  
 * /robot_pose_ekf/vo_used  
 * /robot_state_publisher/publish_frequency  
 * /rosdistro  
 * /rosversion  
 * /turtlebot_laptop_battery/acpi_path  
 * /turtlebot_node/bonus  
 * /turtlebot_node/port  
 * /turtlebot_node/update_rate  
 * /use_sim_time  
  
NODES  
  /  
    cmd_vel_mux (nodelet/nodelet)  
    diagnostic_aggregator (diagnostic_aggregator/aggregator_node)  
    kinect_breaker_enabler (create_node/kinect_breaker_enabler.py)  
    mobile_base_nodelet_manager (nodelet/nodelet)  
    robot_pose_ekf (robot_pose_ekf/robot_pose_ekf)  
    robot_state_publisher (robot_state_publisher/robot_state_publisher)  
    turtlebot_laptop_battery (linux_hardware/laptop_battery.py)  
    turtlebot_node (create_node/turtlebot_node.py)  ![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  
 Let's run the commands below: 


```
    $ roslaunch rbx1_bringup turtlebot_minimal_create.launch  
    $ rostopic pub -r 10 /cmd_vel geometry_msgs/Twist '{linear: {x: 0.1, y: 0, z: 0}, angular: {x: 0, y: 0, z: 0.5}}'

```

this will make My Turtlebot\_me (I call mine is ***Turtlebot\_me***) move slowly. so we can observe its nodes and messages.


then use the rqt tool to monitor the nodes and topics.:



```
    $ rqt_graph

```

we get this:


![](https://img-blog.csdn.net/20151123164811832?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


Here we didn't use ekf(Extended Karman Filter). Lets' print out what's in the topic /tf and /robot\_pose\_ekf  
 



```
    $ rostopic echo /tf
    $ rostopic echo /robot_pose_ekf

```

We get below for /tf :   
 



```
    header: 
      seq: 0
      stamp: 
        secs: 1448269102
        nsecs: 722203525
      frame_id: plate_2_link
    child_frame_id: standoff_kinect_1_link
    transform: 
      translation: 
        x: -0.1024382
        y: -0.098
        z: 0.0032004
      rotation: 
        x: 0.0
        y: 0.0
        z: 0.0
        w: 1.0
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```
We can see there is no change except the 
*nsecs* num. 
  
 and: 



```
$ rostopic echo /robot_pose_ekf
WARNING: topic [/robot_pose_ekf] does not appear to be published yet
```
Let's see /joint\_states & /diagnostics : 
  


```
$ rostopic echo /joint_states
header: 
  seq: 395
  stamp: 
    secs: 1448269407
    nsecs: 869920969
  frame_id: ''
name: ['left_wheel_joint', 'right_wheel_joint', 'front_castor_joint', 'back_castor_joint']
position: [0.0, 0.0, 0.0, 0.0]
velocity: [0.0, 0.0, 0.0, 0.0]
effort: [0.0, 0.0, 0.0, 0.0]
---
```
we see the 'position' is all zeros. This may be a proble of my gear wheel. I'll try to fix it now. 
  
 & /diagnostics : 
  


```
header: 
  seq: 2238
  stamp: 
    secs: 1448269540
    nsecs: 681997060
  frame_id: ''
status: 
  - 
    level: 0
    name: TurtleBot Node
    message: RUNNING
    hardware_id: ''
    values: []
  - 
    level: 0
    name: Operating Mode
    message: Full
    hardware_id: ''
    values: []
  - 
    level: 0
    name: Battery
    message: OK
    hardware_id: ''
    values: 
      - 
        key: Voltage (V)
        value: 15.613
      - 
        key: Current (A)
        value: -0.344
      - 
        key: Temperature (C)
        value: 32
      - 
        key: Charge (Ah)
        value: 2.297
      - 
        key: Capacity (Ah)
        value: 2.696
  - 
    level: 0
    name: Charging Sources
    message: None
    hardware_id: ''
    values: []
  - 
    level: 0
    name: Cliff Sensor
    message: OK
    hardware_id: ''
    values: 
      - 
        key: Left
        value: False
      - 
        key: Left Signal
        value: 2475
      - 
        key: Front Left
        value: False
      - 
        key: Front Left Signal
        value: 2561
      - 
        key: Front Right
        value: False
      - 
        key: Front Right Signal
        value: 2606
      - 
        key: Right
        value: False
      - 
        key: Right Signal
        value: 2609
  - 
    level: 0
    name: Wall Sensor
    message: OK
    hardware_id: ''
    values: 
      - 
        key: Wall
        value: False
      - 
        key: Wall Signal
        value: 61
      - 
        key: Virtual Wall
        value: False
  - 
    level: 2
    name: Gyro Sensor
    message: Gyro Power Problem: For more information visit http://answers.ros.org/question/2091/turtlebot-bad-gyro-calibration-also.
    hardware_id: ''
    values: 
      - 
        key: Gyro Enabled
        value: True
      - 
        key: Raw Gyro Rate
        value: 0
      - 
        key: Calibration Offset
        value: 0
      - 
        key: Calibration Buffer
        value: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
  - 
    level: 0
    name: Digital Outputs
    message: OK
    hardware_id: ''
    values: 
      - 
        key: Raw Byte
        value: 1
      - 
        key: Digital Out 2
        value: ON
      - 
        key: Digital Out 1
        value: OFF
      - 
        key: Digital Out 0
        value: OFF
---
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```
we see a lot of '0's here. This may be a proble of my gear wheel. I'll try to fix it now. 
  

This should be read:http://answers.ros.org/question/31935/createroomba-odometry/?answer=221438#post-id-221438  
 Note this message (from: iRobot\_Roomba\_600\_Open\_Interface\_Spec\_0908.pdf)  
 




```
NOTE: Create 2 and Roomba 500/600 firmware versions prior to 3.3.0 return an incorrect value for 
sensors measured in millimeters. It is recommended that you read the left and right encoder counts 
directly (packets IDs 43 and 44) and do the unit conversion yourself.   
To determine the firmware version on your robot, send a 7 via the serial port to reset it. The robot will 
print a long welcome message which will include the firmware version, for example: 
r3_robot/tags/release-3.3.0. 
```
Looks like i'm going to read the real value of distace and do the unit convert by my self.(mine is 3.2.0) 

The 'Create Open Interface\_v2.pdf' shows us a 25pin connector which is not contained in ROOMBA600/Create2.   
 ![](https://img-blog.csdn.net/20151124180216519?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 



Now the Tasks are:


1. First read out the 'packets IDs 43 and 44' and convert them into distance.
2. Read out the 'packet IDs 43 and 44': angle in radians = (right wheel distance – left wheel distance) / wheel base distance.
3. Understand and modify the base driver, and to let it read packet IDs 43 and 44, and let it convert them into pose and direction.
4. make the base driver publish the pose info as a topic.
5. make all the systems started by the turtlebot\_bringup\_minimal\_me.launch file works well along with that replaced the Gyro data by the Distance data.
6. Add an enternal Gyro Sensor and intergrate it into the systems started by the turtlebot\_bringup\_minimal\_me.launch file. Make the system works well.


Let's get into the 1st one next blog. :)  
 


  
 


  
 


  
 


  
 


  
 





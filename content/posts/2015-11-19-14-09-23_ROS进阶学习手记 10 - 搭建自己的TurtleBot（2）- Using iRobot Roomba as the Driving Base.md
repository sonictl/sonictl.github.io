---
layout: post
title:  "ROS进阶学习手记 10 - 搭建自己的TurtleBot（2）- Using iRobot Roomba as the Driving Base"
date: 2015-11-19 14:09:23 +0800
tags: 
slug: p20151119140923
---

# ROS进阶学习手记 10 - 搭建自己的TurtleBot（2）- Using iRobot Roomba as the Driving Base





## ROS进阶学习手记 10 - 搭建自己的TurtleBot（2）- Using iRobot Roomba as the Driving Base

 使用iRobot Roomba 5xx/6xx/7xx作为turtlebot的驱动底盘 
  

  

 ![](https://img-blog.csdn.net/20151118160803700?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


### 要略过的一些内容：


参考《ROS by Example \_ Hydro \_ Vol1》书中所述，实现到“**7.5  Sending Twist Messages to a Real Robot**”  
 


中间的过程就按书中来进行即可。涉及到的比如设备驱动、ROS网络配置 等内容可参考本博客前的相关文章。


### 要在这里讨论的问题：

          在 
**roslaunch rbx1\_bringup turtlebot\_minimal\_create.launch** 这个命令运行时，PC（my\_robot1）可以接收cmd\_vel消息，并通过串口驱动 Create2® 。 
  

         这里调用的顺序是什么？启动了哪些nodes? 参数是如何设置的？  
          我们运行了-v 选项的 roslaunch 命令以后，得到如下的状态打印：一行行来分析【见文末附录1：turtlebot\_minimal\_create.launch文件的执行记录分析】  
          这里面主要的操作就是2个：


* 1. 设置node运行前需要设置的参数parameters
* 2. 运行nodes


### 关于.launch 文件们


turtlebot\_minimal\_create.launch 这个launch文件是我们驱动iRobot\_Roomba底盘时首先要运行的，显然是很重要的，看看它的层次结构：


#### 1. 标签


首先我们熟悉一下launch文件的tags-标签们：


* <arg name="base"       default="create"/>     
 这是一个argument标签，表示声明一个参数，后面name指定了参数名为“base”, default指定了它的默认值"create"
* <param name="/use\_sim\_time" value="$(arg simulation)"/>  
 这是一个ros\_parameter标签，即ROS程序运行时需要输入的参数，后面name指定了参数名为“/use\_sim\_time”, value指定了它的赋值，此地是调用前面 arg name="simulation" 的argument的值 采用了$ 符号。
* <include file="$(find turtlebot\_bringup)/launch/includes/robot.launch.xml">  
 引用Include标签，意思引用外部的launch文件或xml文件。 $(find package\_name)这里是一个调用ROS外部包的写法，意思robot.launch.xml文件是在turtlebot\_bringup这个ROSpackage下面的。ps:是不是很简单？但是在</include>之前还有很多arg标签，这个是专门为所引xml文件配置参数的用法。
* <node pkg="create\_node" type="turtlebot\_node.py" name="turtlebot\_node" respawn="true" args="--respawnable">  
 node标签，节点标签，这个可能是在某个xml文件中有用到，也很常用。比如tb\_create\_mobile\_base.xml文件里，开了create\_node 这个包下的 turtlebot\_node这个节点。并为它配置了一些属性（respawn-重新生成） ，及参数（如：<param name="sensor\_timeout" value="1.0"/>），还有remap参数，即重映射参数标签
* <remap from="cmd\_vel" to="mobile\_base/commands/velocity" />  
 remap parameter重映射参数标签，就是重新映射一下node运行时的参数名，比如A node发布了 topic\_a 但 B\_node的代码里写的是subcribe到topic\_A, 此时就要把topic\_a重映射成topic\_A ,以便让B\_node能够听到那个topic上的message.
* 别的就涉及到时自己google咯~~


#### 2. 调用关系


  
 


这里我逐层来分析launch 文件：


【turtlebot\_minimal\_create.launch】



> * 配了一堆参数args，指定了base, battery, stacks, 3d\_sensor, simulation;  
>  声明一个参数/use\_sim\_time
> 
> 
> * 引用了一个外部xml文件：【/opt/ros/hydro/share/turtlebot\_bringup/launch/includes/robot.launch.xml】并为它配了参数  
> 
> 	+ 配了一堆参数
> 	+ 又引用了一堆xml文件：$(find turtlebot\_bringup)/launch/includes/description.launch.xml
> 	+ 这里面主要是涉及到对机器人外观的解释-urdf的一些操作，关于urdf需要读者以后自行去学习ROS wiki，暂时不展开。
> 	+ 除此之外，还配了关于robot\_name/robot\_type 等的参数，涉及到启动俩nodes: robot\_state\_publisher&diagnostic\_aggregator  
> 	
> 		- 具体可以搜它们的功能，还涉及到yaml配置文件里配了很多与这俩节点有关的参数，后面会慢慢学到yaml的用法。
> 	+
> 	+
> * 引用了一个外部xml文件：【/home/exbot/catkin\_ws/src/rbx1/rbx1\_bringup/launch/includes/tb\_create\_mobile\_base.launch.xml】并为它配了参数  
>  
> 
> 
> 	+ 一来就启动了一个重要的node: node pkg="create\_node" type="turtlebot\_node.py" name="turtlebot\_node"  
> 	
> 		- 就是它负责调用create\_driver这个驱动程序（后面会学到），负责和驱动程序通信，也和上层那些功能nodes通信，承上启下的作用。  
> 		 很重要。
> 	+ 又来启动一个node: <node pkg="create\_node" type="kinect\_breaker\_enabler.py" name="kinect\_breaker\_enabler"/>  
> 	
> 		- 引用orangeheadlxm的评论：  
> 		  kinect\_breaker\_enabler.py里是写了一个客户端，其对应的服务器写在了turtlebot\_node.py里。它们的作用是当irobot通过SCI和上位机连接上时，把irobot设置为full模式，若未连接上则返回错误并且客户端继续向服务器请求。把response = service\_proxy(3)中的3改为1或2还可以把irobot设置成其他模式。
> 	+ 又来启动一个node:<node pkg="robot\_pose\_ekf" type="robot\_pose\_ekf" name="robot\_pose\_ekf">  
> 	
> 		- 这是关于robot\_pose\_ekf包的内容，自行wiki，主要意思是使用odometry + gyroSensor的数据来估计机器人的位置。
> 		- refer: <http://wiki.ros.org/robot_pose_ekf>
> 		- When you're using ROOMBA rather than the CREATE as the moving base of your turtlebot, you need to read this wiki before adding your own gyro sensor.  
> 		 也很重要！
> 	+ 又来启动一个node:<node pkg="nodelet" type="nodelet" name="mobile\_base\_nodelet\_manager" args="manager"/>  
> 	
> 		- 这是nodelet包下的内容，这里不展开，主要用于 velocity commands multiplexer
> 		- 与它平行还启动了一个node：<node pkg="nodelet" type="nodelet" name="cmd\_vel\_mux"，这里不展开，主要用于 velocity commands multiplexer
> 	+ 与上一个node平行还启动了一个node：<node pkg="nodelet" type="nodelet" name="cmd\_vel\_mux"，  
> 	
> 		- 这里不展开，主要用于 velocity commands multiplexer
> 		- bla..
> 	+ blabla..
> * 引用了一个外部xml文件：【/opt/ros/hydro/share/turtlebot\_bringup/launch/includes/netbook.launch.xml】并为它配了参数  
>  
> 
> 
> 	+ 配了有关电池的参数，我想是用来管理电源的
> 	+ 引用了一个外部xml文件：【/opt/ros/hydro/share/turtlebot\_bringup/launch/includes/netbook.launch.xml】  
> 	
> 		- 配了一个参数arg name="battery"
> 		- 启动一个node:<node pkg="linux\_hardware" type="laptop\_battery.py" name="turtlebot\_laptop\_battery">,配了相关参数，这里就不展开了。
> 	+ 
> 	+


至此，我们就细看完了 tb\_create\_mobile\_base.launch.xml 这个文件， 一个驱动、一个odometry estimator配置，一个velocity commands multiplexer选择器。  
          具体的讨论见**附录2：关于turtlebot\_minimal\_create.launch 文件运行详解**:


完成了底盘驱动，我们有几个问题没有解决：


1. 驱动速度是否按照命令输入的进行？ -- 速度驱动校准，参见附录3： tb\_create\_mobile\_base.launch.xml文件的修改（添加速度校准参数）
2. 由于我们采用的ROOMBA而不是IRobot\_Create1, 所以没有了gyro\_Sensor 数据用于 ekf 的位置估计，ROS利用roomba底盘马达上自带的码盘进行了位置估计，参见odometry相关wiki。我们希望能添加extra gyro sensor, 具体见ros 社区的讨论：<http://answers.ros.org/question/221866/how-to-add-extra-gyro-sensor-for-roomba/>
3. square 驱动实验，看能否准确。参见ros\_by\_example\_hydro\_vol1: *7.9  Navigating a Square using Odometry*



至此，我们就分析完了**turtlebot\_minimal\_create.launch** 文件**~~**


***--Edited by Sonictl @ csdn blog*  
 *on July 3rd, 2016***  
 


  


### 附录1：turtlebot\_minimal\_create.launch文件的执行记录分析】




```
$ roslaunch -v rbx1_bringup turtlebot_minimal_create.launch
... logging to /home/exbot/.ros/log/d0c59c34-8dba-11e5-a599-a021b74eea2b/roslaunch-my_robot1-10296.log  //这是加载日志文件
Checking log directory for disk usage. This may take awhile.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

... loading XML file [/opt/ros/hydro/etc/ros/roscore.xml]    //第一个加载的xml文件，是运行roscore, 略过。
... executing command param [rosversion roslaunch]
Added parameter [/rosversion]
... executing command param [rosversion -d]  
Added parameter [/rosdistro]                                 // 前面这几行都是获取 ROS 的版本 = hydro
Added core node of type [rosout/rosout] in namespace [/] 
... loading XML file [/home/exbot/catkin_ws/src/rbx1/rbx1_bringup/launch/turtlebot_minimal_create.launch]   //1.从这里开始加载launch文件了
Added parameter [/use_sim_time]       //上一个文件的第8行“<param name="/use_sim_time" value="$(arg simulation)"/>”，开始设置参数值。
... loading XML file [/opt/ros/hydro/share/turtlebot_bringup/launch/includes/robot.launch.xml]          // 2.加载robot.launch.xml 文件
... loading XML file [/opt/ros/hydro/share/turtlebot_bringup/launch/includes/description.launch.xml]    // 3.加载description.launch.xml文件
... executing command param [/opt/ros/hydro/share/xacro/xacro.py '/opt/ros/hydro/share/turtlebot_description/robots/create_circles_kinect.urdf.xacro']
Added parameter [/robot_description]
... done importing include file [/opt/ros/hydro/share/turtlebot_bringup/launch/includes/description.launch.xml]
Added parameter [/robot/name]     //来自：【loading XML file [/opt/ros/hydro/share/turtlebot_bringup/launch/includes/robot.launch.xml]】
Added parameter [/robot/type]     //来自：【loading XML file [/opt/ros/hydro/share/turtlebot_bringup/launch/includes/robot.launch.xml]】
Added parameter [/robot_state_publisher/publish_frequency]
Added node of type [robot_state_publisher/robot_state_publisher] in namespace [/]  //在【launch/includes/robot.launch.xml】文件中启动node:[robot_state_publisher]

Added parameter [/diagnostic_aggregator/analyzers/digital_io/path] //【载入pkg=diagnostic_aggregator nodename=diagnostic_aggregator,载入参数文件/opt/ros/hydro/share/turtlebot_bringup/param/create/diagnostics.yaml】
Added parameter [/diagnostic_aggregator/analyzers/digital_io/type]//1【参数文件中path,type,timeout,startswitch】
Added parameter [/diagnostic_aggregator/analyzers/digital_io/timeout]
Added parameter [/diagnostic_aggregator/analyzers/digital_io/startswith]

Added parameter [/diagnostic_aggregator/analyzers/sensors/path]   //2【参数文件中path,type,timeout,startswitch】
Added parameter [/diagnostic_aggregator/analyzers/sensors/type]
Added parameter [/diagnostic_aggregator/analyzers/sensors/timeout]
Added parameter [/diagnostic_aggregator/analyzers/sensors/startswith]

Added parameter [/diagnostic_aggregator/analyzers/nodes/path]    //3【参数文件中path,type,timeout,CONTAINS】
Added parameter [/diagnostic_aggregator/analyzers/nodes/contains]
Added parameter [/diagnostic_aggregator/analyzers/nodes/type]
Added parameter [/diagnostic_aggregator/analyzers/nodes/timeout]

Added parameter [/diagnostic_aggregator/analyzers/power/path]    //4【参数文件中path,type,timeout,startswitch】
Added parameter [/diagnostic_aggregator/analyzers/power/type]
Added parameter [/diagnostic_aggregator/analyzers/power/timeout]
Added parameter [/diagnostic_aggregator/analyzers/power/startswith]

Added parameter [/diagnostic_aggregator/analyzers/mode/path]     //5【参数文件中path,type,timeout,startswitch】
Added parameter [/diagnostic_aggregator/analyzers/mode/type]
Added parameter [/diagnostic_aggregator/analyzers/mode/timeout]
Added parameter [/diagnostic_aggregator/analyzers/mode/startswith]

Added parameter [/diagnostic_aggregator/pub_rate]                //6【参数文件中pub_rate,base_path,timeout,CONTAINS】
Added parameter [/diagnostic_aggregator/base_path]

Added node of type [diagnostic_aggregator/aggregator_node] in namespace [/] //相关参数载入完成，在【launch/includes/robot.launch.xml】文件中启动node:[diagnostic_aggregator]
... done importing include file [/opt/ros/hydro/share/turtlebot_bringup/launch/includes/robot.launch.xml]  //完成：【loading XML file [/opt/ros/hydro/share/turtlebot_bringup/launch/includes/robot.launch.xml]】完成载入

//进入：【loading XML file [/home/exbot/catkin_ws/src/rbx1/rbx1_bringup/launch/includes/tb_create_mobile_base.launch.xml]】
... loading XML file [/home/exbot/catkin_ws/src/rbx1/rbx1_bringup/launch/includes/tb_create_mobile_base.launch.xml]
Added parameter [/turtlebot_node/bonus]
Added parameter [/turtlebot_node/update_rate]
Added parameter [/turtlebot_node/port]
Added node of type [create_node/turtlebot_node.py] in namespace [/]  //相关参数载入完成，在【tb_create_mobile_base.launch.xml】文件中启动node:[create_node/turtlebot_node.py]
Added node of type [create_node/kinect_breaker_enabler.py] in namespace [/]  // 载入 node: kinect_breaker_enabler.py

Added parameter [/robot_pose_ekf/freq]
Added parameter [/robot_pose_ekf/sensor_timeout]
Added parameter [/robot_pose_ekf/publish_tf]
Added parameter [/robot_pose_ekf/odom_used]
Added parameter [/robot_pose_ekf/imu_used]
Added parameter [/robot_pose_ekf/vo_used]
Added parameter [/robot_pose_ekf/output_frame]
Added node of type [robot_pose_ekf/robot_pose_ekf] in namespace [/] //【载入node】: pkg="robot_pose_ekf" type="robot_pose_ekf" name="robot_pose_ekf"

Added node of type [nodelet/nodelet] in namespace [/] //【载入node】//node pkg="nodelet" type="nodelet" name="mobile_base_nodelet_manager" args="manager"

Added parameter [/cmd_vel_mux/yaml_cfg_file]   //[param name="yaml_cfg_file" value="$(find turtlebot_bringup)/param/mux.yaml"]
//# Configuration for subscribers to multiple cmd_vel sources.  //filepath: /opt/ros/hydro/share/turtlebot_bringup/param/mux.yaml
Added node of type [nodelet/nodelet] in namespace [/] //【载入node】pkg="nodelet" type="nodelet" name="cmd_vel_mux"

... done importing include file [/home/exbot/catkin_ws/src/rbx1/rbx1_bringup/launch/includes/tb_create_mobile_base.launch.xml]

... loading XML file [/opt/ros/hydro/share/turtlebot_bringup/launch/includes/netbook.launch.xml] //【进入[netbook.launch.xml]文件】
Added parameter [/turtlebot_laptop_battery/acpi_path]
Added node of type [linux_hardware/laptop_battery.py] in namespace [/]  //【载入node】pkg="linux_hardware" type="laptop_battery.py" name="turtlebot_laptop_battery"
... done importing include file [/opt/ros/hydro/share/turtlebot_bringup/launch/includes/netbook.launch.xml]

started roslaunch server http://my_robot1:58659/

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
    turtlebot_node (create_node/turtlebot_node.py)

auto-starting new master
process[master]: started with pid [10313]
ROS_MASTER_URI=http://my_robot1:11311

setting /run_id to d0c59c34-8dba-11e5-a599-a021b74eea2b
process[rosout-1]: started with pid [10326]
started core service [/rosout]
process[robot_state_publisher-2]: started with pid [10339]
process[diagnostic_aggregator-3]: started with pid [10360]
process[turtlebot_node-4]: started with pid [10379]
process[kinect_breaker_enabler-5]: started with pid [10443]
process[robot_pose_ekf-6]: started with pid [10447]
process[mobile_base_nodelet_manager-7]: started with pid [10487]
process[cmd_vel_mux-8]: started with pid [10508]
process[turtlebot_laptop_battery-9]: started with pid [10541]
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  


### 附录2： 关于turtlebot\_minimal\_create.launch 文件运行详解

 从roslaunch -v 的状态打印来看，系统主要做了两方面的工作： 
  

1. Added parameter. Generated by **<param name="" value=""/>**
2. executing command param. Generated by **<param name="" command="$(arg arg\_name)"/>**
3. loading XML file
4. added node of type [xx] in namespace


  
 先看Add parameter, 这是由.launch文件或者.xml 文件中 <param name="" value="" /> tag attributed. 
  
 We also see <arg name="" default="" /> Tag. 
  
 Q:What's the difference between parameter and argrement? 
  

  
 Let's see the Stackflow, it says: 
  
 ============= 
  
      A Parameter is a variable in the declaration of a function: 
  

  

    functionName(parameter) {  
     // do something  
     }
  

  

  
     An Argument is the actual value of this variable that gets passed to the function: 
  

  

    functionName(argument);
  
 ============= 
  
 I modified the turtlebot\_minimal\_create.launch file and saved it as 
**turtlebot\_minimal\_create\_me.launch**
  

*Let's see the turtlebot\_minimal\_create\_me.launch File line by line below:*

```
1. <launch>
2. <arg name="simulation" default="$(optenv TURTLEBOT_SIMULATION false)"/><!-- For param "/use_sim_time" -->
3. <!--Next three lines are for .../launch/includes/robot.launch.xml-->
4. <arg name="base"       default="roomba"/>   <!-- create, roomba, kobuki -->
5. <arg name="stacks"     default="circles"/>  <!-- circles, hexagons -->
6. <arg name="3d_sensor"  default="asus_xtion_pro"/>  <!-- kinect, asus_xtion_pro -->
7. <!--Next line is for .../launch/includes/netbook.launch.xml-->
8. <arg name="battery"    default="$(optenv TURTLEBOT_BATTERY /proc/acpi/battery/BAT0)"/>  <!-- /proc/acpi/battery/BAT0 -->
9. 
10. <param name="/use_sim_time" value="$(arg simulation)"/> <!--Added parameter /use_sim_time-->
11. 
12. <include file="$(find turtlebot_bringup)/launch/includes/robot.launch.xml">
13. <arg name="base" value="$(arg base)" />       <!--Plugged in three arguments:base; stacks; 3d_sensor-->
14. <arg name="stacks" value="$(arg stacks)" />
15. <arg name="3d_sensor" value="$(arg 3d_sensor)" />
16. </include>
17. 
18. <!--Below: Turtlebot Driver/ breaker1for kinect/ odometry estimator/ vel_com_multiplexer-->
19. <include file="$(find rbx1_bringup)/launch/includes/tb_create_mobile_base.launch.xml" />
20. 
21. <!--Netbook battery monitor-->
22. <include file="$(find turtlebot_bringup)/launch/includes/netbook.launch.xml">
23. <arg name="battery" value="$(arg battery)" />
24. </include>
25. </launch>
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```
   Conclusion: This launch file defined 1 argument "simulation", 3 args "base","stacks","3d\_sensor" for robot description, 1 arg "battery" for netbook.xml as battery monitor.(I didn't use netbook.) , Called three .launch.xml files to finish work: 
* /opt/ros/hydro/share/turtlebot\_bringup/launch/includes/robot.launch.xml
* /home/exbot/catkin\_ws/src/rbx1/rbx1\_bringup/launch/includes/tb\_create\_mobile\_base.launch.xml
* /opt/ros/hydro/share/turtlebot\_bringup/launch/includes/netbook.launch.xml



   I'll first look into the 2nd, **tb\_create\_mobile\_base.launch.xml** to set the parameters as my turtlebot's real situation. And see how it works.  
 


#### **tb\_create\_mobile\_base.launch.xml**


  



```
1. <launch>
2. <!-- Turtlebot Driver -->
3. <node pkg="create_node" type="turtlebot_node.py" name="turtlebot_node" respawn="true" args="--respawnable">
4. <param name="bonus" value="false" />
5. <param name="update_rate" value="30.0" />
6. <param name="port" value="/dev/ttyUSB0" />
7. <remap from="cmd_vel" to="mobile_base/commands/velocity" />
8. <remap from="turtlebot_node/sensor_state" to="mobile_base/sensors/core" />
9. <remap from="imu/data" to="mobile_base/sensors/imu_data" />
10. <remap from="imu/raw" to="mobile_base/sensors/imu_data_raw" />
11. </node>
12. 
13. <!-- Enable breaker 1 for the kinect -->
14. <node pkg="create_node" type="kinect_breaker_enabler.py" name="kinect_breaker_enabler"/>
15. 
16. <!-- The odometry estimator -->
17. <node pkg="robot_pose_ekf" type="robot_pose_ekf" name="robot_pose_ekf">
18. <remap from="imu_data" to="mobile_base/sensors/imu_data"/>
19. <remap from="robot_pose_ekf/odom" to="odom_combined"/>
20. <param name="freq" value="10.0"/>
21. <param name="sensor_timeout" value="1.0"/>
22. <param name="publish_tf" value="true"/>
23. <param name="odom_used" value="true"/>
24. <param name="imu_used" value="true"/>
25. <param name="vo_used" value="false"/>
26. <param name="output_frame" value="odom"/>
27. </node>
28. 
29. <!-- velocity commands multiplexer -->
30. <node pkg="nodelet" type="nodelet" name="mobile_base_nodelet_manager" args="manager"/>
31. <node pkg="nodelet" type="nodelet" name="cmd_vel_mux" args="load yocs_cmd_vel_mux/CmdVelMuxNodelet mobile_base_nodelet_manager">
32. <param name="yaml_cfg_file" value="$(find turtlebot_bringup)/param/mux.yaml"/>
33. <remap from="cmd_vel_mux/output" to="mobile_base/commands/velocity"/>
34. <remap from="cmd_vel_mux/input/navi" to="cmd_vel"/>
35. </node>
36. </launch>
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```
第一部分： 

```
1. <!-- Turtlebot Driver -->
2. <node pkg="create_node" type="turtlebot_node.py" name="turtlebot_node" respawn="true" args="--respawnable">
3. <param name="bonus" value="false" />
4. <param name="update_rate" value="30.0" />
5. <param name="port" value="/dev/ttyUSB0" />
6. <remap from="cmd_vel" to="mobile_base/commands/velocity" />
7. <remap from="turtlebot_node/sensor_state" to="mobile_base/sensors/core" />
8. <remap from="imu/data" to="mobile_base/sensors/imu_data" />
9. <remap from="imu/raw" to="mobile_base/sensors/imu_data_raw" />
10. </node>

```
这里先为node = turtlebot\_node.py 定义了三个parameter: bonus/ update\_rate/ port , 然后 remap 了4个消息名。最后启动 turtlebot\_node.py ，实际是个底盘驱动。 
  

  

第二部分：



```
1. <!-- Enable breaker 1 for the kinect -->
2. <node pkg="create_node" type="kinect_breaker_enabler.py" name="kinect_breaker_enabler"/>

```
 查一下 kinect\_breaker\_enabler.py 是个什么东东？ 
  

    进入到 kinect\_breaker\_enabler.py 代码里面发现是 Set\_to\_full\_Mode ，看来是个设置底板模式的功能。别的涉及到proxy, server啥的，看不懂。~\_~


    2016年5月6日17:05:39补充一大牛的回复：[orangeheadlxm：](http://blog.csdn.net/orangehead1)




kinect\_breaker\_enabler.py里是写了一个客户端，其对应的服务器写在了turtlebot\_node.py里。它们的作用是当irobot通过SCI和上位机连接上时，把irobot设置为full模式，若未连接上则返回错误并且客户端继续向服务器请求。把response = service\_proxy(3)中的3改为1或2还可以把irobot设置成其他模式。


  

  

第三部分：



```
1. <!-- The odometry estimator -->
2. <node pkg="robot_pose_ekf" type="robot_pose_ekf" name="robot_pose_ekf">
3. <remap from="imu_data" to="mobile_base/sensors/imu_data"/>
4. <remap from="robot_pose_ekf/odom" to="odom_combined"/>
5. <param name="freq" value="10.0"/>
6. <param name="sensor_timeout" value="1.0"/>
7. <param name="publish_tf" value="true"/>
8. <param name="odom_used" value="true"/>  <!-- Sonictl Set as false -->
9. <param name="imu_used" value="true"/>   <!-- Sonictl Set as false -->
10. <param name="vo_used" value="false"/>
11. <param name="output_frame" value="odom"/>
12. </node>

```
  这里要根据我的情况修改几个参数设置了。我没有IMU的 ekf 功能，没有搭载 accelerometer, 
 也不知道怎么搭载。所以就把某些值设为false.再保存到tb\_create\_mobile\_base.launch\_me.xml 中 
  

第四部分：



```
1. <!-- velocity commands multiplexer -->
2. <node pkg="nodelet" type="nodelet" name="mobile_base_nodelet_manager" args="manager"/>
3. <node pkg="nodelet" type="nodelet" name="cmd_vel_mux" args="load yocs_cmd_vel_mux/CmdVelMuxNodelet mobile_base_nodelet_manager">
4. <param name="yaml_cfg_file" value="$(find turtlebot_bringup)/param/mux.yaml"/>
5. <!-- filepath: /opt/ros/hydro/share/turtlebot_bringup/param/mux.yaml -->
6. <remap from="cmd_vel_mux/output" to="mobile_base/commands/velocity"/>
7. <remap from="cmd_vel_mux/input/navi" to="cmd_vel"/>
8. </node>

```
  这一部分是 多cmd\_vel消息来了时候，进行multiplexer多路转换器，的功能。 这里先设置了一个parameter: yaml\_cfg\_file配置文件，设置到【/opt/ros/hydro/share/turtlebot\_bringup/param/mux.yaml】，然后开了俩node：nodelet/mobile\_base\_nodelet\_manager  &  nodelet/cmd\_vel\_mux , 都分别带了参数args. 
  
   最后是remap 了俩消息。 
  
   来看看yaml的配置文件： 
  


```
# Created on: Oct 29, 2012
#     Author: jorge
# Configuration for subscribers to multiple cmd_vel sources.
#
# Individual subscriber configuration:
#   name:           Source name
#   topic:          The topic that provides cmd_vel messages
#   timeout:        Time in seconds without incoming messages to consider this topic inactive
#   priority:       Priority: an UNIQUE unsigned integer from 0 (lowest) to MAX_INT 
#   short_desc:     Short description (optional)

subscribers:
  - name:        "Safe reactive controller"
    topic:       "input/safety_controller"
    timeout:     0.2
    priority:    10
  - name:        "Teleoperation"
    topic:       "input/teleop"
    timeout:     1.0
    priority:    7
  - name:        "Navigation"
    topic:       "input/navi"
    timeout:     1.0
    priority:    5![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```
  可见是设置了三个源的优先级。文件开头也写了： Configuration for subscribers to multiple cmd\_vel sources. 
  至此，我们就细看完了 tb\_create\_mobile\_base.launch.xml 这个文件，一个驱动、一个odometry estimator配置，一个velocity commands multiplexer选择器。


  完成了驱动，我们有几个问题没有解决：


1. 驱动速度是否按照命令输入的进行？ -- 速度驱动校准
2. 没有了 ekf 的位置估计，如何利用Create底盘马达上自带的码盘进行位置估计？
3. square 驱动实验，看能否准确。


### 附录3： tb\_create\_mobile\_base.launch.xml文件的修改（添加速度校准参数）




```
1. <!--Sonic add these parameters below:  -->
2. <param name="has_gyro" value="true" />
3. <param name="turtlebot_node/odom_angular_scale_correction" value="1.03"/>
4. <param name="turtlebot_node/odom_linear_scale_correction" value="1.0"/>
5. <param name="robot_type" value="roomba"/> <!--"create" or "roomba" 这里很重要！！-->
6. <!--Sonic add these parameters above.  -->

```

  


  
 


  
 


  
 


  
 





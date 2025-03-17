---
layout: post
title:  "ROS 进阶学习笔记（17）：ROS导航2：关于 move_base Package(底盘移动包)"
date: 2016-06-02 16:46:06 +0800
tags: 
slug: p20160602164606
---

# ROS 进阶学习笔记（17）：ROS导航2：关于 move_base Package(底盘移动包)





## == 关于move\_base 包(底盘移动包？移动底盘包？) ==


开始之前，我sonictl@CSDN有几个问题([Link on ROS\_Answer](http://answers.ros.org/question/235642/hesitate-when-turtlebot-meet-the-obstacles/))需要搞定：


1. costmap\_2d包 与 move\_base包 是什么关系？
2. 导航时，在RviZ工具中，可以看到，inflate 地图是由一个叫做 /move\_base/local\_costmap/costmap 主题生成的，这个主题的发布者又是move\_base，怎么发布的？
3. 到底是costmap\_2d发布的/move\_base/local\_costmap/costmap还是move\_base发布的它？又回到第1个问题，关系问题。
4. <http://wiki.ros.org/costmap_2d/hydro/obstacles> 这里是不是可以探寻一点东西出来，以调整obstacle层的刷新机制
5. 我sonictl@CSDN要解决的就是inflated obstacle层的刷新机制
6. 


How to solve the problem:


![](https://img-blog.csdn.net/20160601135807681?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


  
 


=== 我学这个包的时候，尽量总结wiki page上的内容如下，算是sonictl@CSDN读书笔记：===  
 wikipage: <http://wiki.ros.org/move_base>  
 [move\_base](http://wiki.ros.org/move_base) [package](https://so.csdn.net/so/search?q=package&spm=1001.2101.3001.7020) 所属Stack: navigation  
 关于这个包在 ROS Navigation 框架中的位置，参见：[ROS探索总结（十三）——导航与定位框架](http://blog.csdn.net/hcx25909/article/details/9334231)  
 


### 【Summary】


The move\_base package provides an implementation of an action (see the  [actionlib](http://www.ros.org/wiki/actionlib) package) that, given a goal in the world, will attempt to reach it with a mobile base. The move\_base [node](https://so.csdn.net/so/search?q=node&spm=1001.2101.3001.7020) links together a global and local planner to accomplish its global navigation task. It supports any global planner adhering to the nav\_core::BaseGlobalPlanner interface specified in the  [nav\_core](http://www.ros.org/wiki/nav_core) package and any local planner adhering to the nav\_core::BaseLocalPlanner interface specified in the[nav\_core](http://www.ros.org/wiki/nav_core) package. The move\_base node also maintains two costmaps, one for the global planner, and one for a local planner (see the[costmap\_2d](http://www.ros.org/wiki/costmap_2d) package) that are used to accomplish navigation tasks.


这个包就是一个Action的实现，关于action，是一个ROS里的概念，需要到[链接](http://www.ros.org/wiki/actionlib)去学习。这个action就是要有几个输入，比如goal,然后它要给出的就是你的mobile base怎么到那里去的一个action输出。the move\_base node实际上是一个link, 把全局规划器、局部规划器连接起来，以达成导航到目的地的目标。nav\_core::BaseGlobalPlanner 接口里由[nav\_core](http://www.ros.org/wiki/nav_core)指定的那些全局或 nav\_core::BaseLocalPlanner 接口里由[nav\_core](http://www.ros.org/wiki/nav_core)指定的局部规划器都被 move\_base 支持。move\_base node 还维护了俩costmap, 一个是为全局规划器准备的 global costmap, 一个是local的，这俩costmap也是为了完成sonictl@CSDN导航的任务。


  
 



### 【关于 move\_base Nodes】


This package provides the move\_base ROS Node which is a major component of the[navigation stack](http://wiki.ros.org/navigation). A detailed description of this Node and its configuration options is found below. 主要的结构就是见下图咯：


![](http://wiki.ros.org/move_base?action=AttachFile&do=get&target=overview_tf_small.png)  
 


    要说的呢，就是move\_base看起来仿佛是个壳儿，壳儿负责和外界交流，它肚子里的才是干货~~sonictl@CSDN呵呵！


    The move\_base node provides a ROS interface for configuring, running, and interacting with the[navigation stack](http://wiki.ros.org/navigation) on a robot. A high-level view of the move\_base node and its interaction with other components is shown above. The blue vary based on the robot platform, the gray are optional but are provided for all systems, and the white nodes are required but also provided for all systems. For more information on configuration of themove\_base node, and the navigation stack as a whole, please see the[navigation setup and configuration](http://wiki.ros.org/navigation/Tutorials/RobotSetup) tutorial.


    这里讲了各个颜色的方框代表的含义，给了一个tutorial的链接，教你怎么配置 navigation setup and configuration. 配置导航stack肯定会用到的[link](http://wiki.ros.org/navigation/Tutorials/RobotSetup)  
 


      
 


【期望的机器人的行为】


先扔了一幅图：


![](http://wiki.ros.org/move_base?action=AttachFile&do=get&target=recovery_behaviors.png)



Running the move\_base node on a robot that is properly configured (please see[navigation stack documentation](http://wiki.ros.org/navigation) for more details) results in a robot that will attempt to achieve a goal pose with its base to within a user-specified tolerance. In the absence of dynamic obstacles, the move\_base node will eventually get within this tolerance of its goal or signal failure to the user. Themove\_base node may optionally perform recovery behaviors when the robot perceives itself as stuck. By default, the[move\_base](http://wiki.ros.org/move_base) node will take the following actions to attempt to clear out space(sonictl@CSDN):

 First, obstacles outside of a user-specified region will be cleared from the robot's map. Next, if possible, the robot will perform an in-place rotation to clear out space. If this too fails, the robot will more aggressively clear its map, removing all obstacles outside of the rectangular region in which it can rotate in place. This will be followed by another in-place rotation. If all this fails, the robot will consider its goal infeasible and notify the user that it has aborted. These recovery behaviors can be configured using the 
 [recovery\_behaviors](http://wiki.ros.org/move_base#Parameters) parameter, and disabled using the 
[recovery\_behavior\_enabled](http://wiki.ros.org/move_base#Parameters) parameter. 
对图作一下解释： 在你已经配置得比较好的机器人上运行 move\_base node就会让你的机器人跑到指定的位姿去，在tolerance,容错值允许的范围内到达。In the absence of dynamic obstacles，要么 move\_base node会最终到达，要么会返回一个失败信号。当机器人感觉被卡住stuck时，the move\_base node可能会选择性地执行recovery 恢复行为。默认的，这个 move\_base node 会采取以下的措施来clear out space: 搞清楚自己的处境？


* 首先，机器人清扫地图，无关紧要的障碍就不处理了。
* 然后，机器人会原地转圈来搞清楚自己处境clear out space
* 如果这个失败，机器人会变得更勇敢地clear its map, 移除矩形区域以外的所有的障碍，而这个矩形区域是它可以在其中原地转圈的区域。
* 接下来就是另一个 in-place 转圈原地转圈。
* 如果上述所有步骤都失败了，机器人可能会考虑它的goal达不到了并通知用户aborted任务失败。

         sonictl@CSDN: 这些恢复行为可以在 the 
 [recovery\_behaviors](http://wiki.ros.org/move_base#Parameters) parameter 里配置，或者开关这些行为：and be disabled using the 
 [recovery\_behavior\_enabled](http://wiki.ros.org/move_base#Parameters) parameter 
  

  
 


### 【Action API】


The move\_base node provides an implementation of theSimpleActionServer (see[actionlib documentation](http://wiki.ros.org/actionlib)), that takes in goals containinggeometry\_msgs/PoseStamped messages. You can communicate with themove\_base node over ROS directly, but the recommended way to send goals tomove\_base if you care about tracking their status is by using theSimpleActionClient. Please see  [actionlib documentation](http://wiki.ros.org/actionlib) for more information.


The move\_base node 提供的是一个SimpleActionServer (see[actionlib documentation](http://wiki.ros.org/actionlib))的动作服务器，它吃进 goals containinggeometry\_msgs/PoseStamped messages。你可以在ROS架构上和move\_base node 通信，但如果你关心它们的状态的话，推荐的方式是通过使用 theSimpleActionClient给它发goals, 详见[actionlib documentation](http://wiki.ros.org/actionlib) for more information。（看来 actionlib documentation 是必须要搞一搞才行了~~）  
 


sonictl: 这里面涉及到ROS里的几个值得总结的概念分类问题: 1. What is Service/Actionlib/plugin...     2. What is message/topic/parameter/node/tf   3. ...other concepts  
 



#### Action Subscribed Topics:




move\_base/goal ([move\_base\_msgs/MoveBaseActionGoal](http://docs.ros.org/api/move_base_msgs/html/msg/MoveBaseActionGoal.html))


* A goal for move\_base to pursue in the world.


move\_base/cancel ( 
[actionlib\_msgs/GoalID](http://docs.ros.org/api/actionlib_msgs/html/msg/GoalID.html)) 

* A request to cancel a specific goal.





#### Action Published Topics:




move\_base/feedback ([move\_base\_msgs/MoveBaseActionFeedback](http://docs.ros.org/api/move_base_msgs/html/msg/MoveBaseActionFeedback.html))


* Feedback contains the current position of the base in the world.


move\_base/status ( 
[actionlib\_msgs/GoalStatusArray](http://docs.ros.org/api/actionlib_msgs/html/msg/GoalStatusArray.html)) 

* Provides status information on the goals that are sent to themove\_base action.


move\_base/result ( 
[move\_base\_msgs/MoveBaseActionResult](http://docs.ros.org/api/move_base_msgs/html/msg/MoveBaseActionResult.html)) 
Result is empty for the 
move\_base action. 
  

  
 



### 【Subscribed Topics】


move\_base\_simple/goal ( 
[geometry\_msgs/PoseStamped](http://docs.ros.org/api/geometry_msgs/html/msg/PoseStamped.html)) 
Provides a non-action interface to move\_base for users that don't care about tracking the execution status of their goals. 


### 【Published Topics】



cmd\_vel ([geometry\_msgs/Twist](http://docs.ros.org/api/geometry_msgs/html/msg/Twist.html))A stream of velocity commands meant for execution by a mobile base.  
 



### 【Services】



这几个services还是挺有用的，我sonictl就不纯翻译了。sonictl@CSDN  
 


~make\_plan ([nav\_msgs/GetPlan](http://docs.ros.org/api/nav_msgs/html/srv/GetPlan.html))


* Allows an external user to ask for a plan to a given pose frommove\_base without causingmove\_base to execute that plan.  请求一个plan, 但它不执行


~clear\_unknown\_space ( 
[std\_srvs/Empty](http://docs.ros.org/api/std_srvs/html/srv/Empty.html)) 

* Allows an external user to tell move\_base to clear unknown space in the area directly around the robot. This is useful when move\_base has its costmaps stopped for a long period of time and then started again in a new location in the environment. - **Available in versions from 1.1.0-groovy**


~clear\_costmaps ( 
[std\_srvs/Empty](http://docs.ros.org/api/std_srvs/html/srv/Empty.html)) 
Allows an external user to tell move\_base to clear obstacles in the costmaps used by move\_base. This could cause a robot to hit things and should be used with caution. - 
**New in 1.3.1**
  



### 【Parameters】



这些参数一般是写在 ~/ws\_catkin/rbx1/rbx1\_nav/config/turtlebot/base\_local\_planner\_params.yaml 文件里（MyBook: ros\_by\_example\_hydro\_volume1）  
 


~base\_global\_planner (string, default:"navfn/NavfnROS"**For 1.1+ series**)


* The name of the plugin for the global planner to use withmove\_base, see[pluginlib](http://wiki.ros.org/pluginlib) documentation for more details on plugins. This plugin must adhere to thenav\_core::BaseGlobalPlanner interface specified in the[nav\_core](http://wiki.ros.org/nav_core) package. (**1.0 series default:**"NavfnROS")  
 全局规划器的名字，让move\_base拿来用的，关于pluginlib, 要看[文档](http://wiki.ros.org/pluginlib)。这个plugin 必须要粘在nave\_core::BaseGlobalPlanner 的接口上使用，[nav\_core](http://wiki.ros.org/nav_core)定义了那些接口。1.0 系列的默认的全局规划器名字是：NavfnROS


~base\_local\_planner ( 
string, default: 
"base\_local\_planner/TrajectoryPlannerROS"
**For 1.1+ series**) 

* The name of the plugin for the local planner to use withmove\_base see[pluginlib](http://wiki.ros.org/pluginlib) documentation for more details on plugins. This plugin must adhere to thenav\_core::BaseLocalPlanner interface specified in the[nav\_core](http://wiki.ros.org/nav_core) package. (**1.0 series default:**"TrajectoryPlannerROS")  
 局部规划器的插件名，关于插件的事情就还是[那个链接](http://wiki.ros.org/pluginlib)了。这个plugin 必须要粘在nave\_core::BaseLocalPlanner 的接口上使用，[nav\_core](http://wiki.ros.org/nav_core)定义了那些接口。1.0 系列的默认的全局规划器名字是：TrajectoryPlannerROS


~recovery\_behaviors ( 
list, default: [{name: conservative\_reset, type: clear\_costmap\_recovery/ClearCostmapRecovery}, {name: rotate\_recovery, type: rotate\_recovery/RotateRecovery}, {name: aggressive\_reset, type: clear\_costmap\_recovery/ClearCostmapRecovery}] 
**For 1.1+ series**) 
 
* A list of recovery behavior plugins to use with  move\_base, see  [pluginlib](http://wiki.ros.org/pluginlib) documentation for more details on plugins. These behaviors will be run whenmove\_base fails to find a valid plan in the order that they are specified. After each behavior completes,move\_base will attempt to make a plan. If planning is successful,move\_base will continue normal operation. Otherwise, the next recovery behavior in the list will be executed. These plugins must adhere to thenav\_core::RecoveryBehavior interface specified in the  [nav\_core](http://wiki.ros.org/nav_core) package. (**1.0 series default:** [{name: conservative\_reset, type: ClearCostmapRecovery}, {name: rotate\_recovery, type: RotateRecovery}, {name: aggressive\_reset, type: ClearCostmapRecovery}]).  
 Note: For the default parameters, the aggressive\_reset behavior will clear out to a distance of 4 \*~/local\_costmap/circumscribed\_radius.  
 这个就是局部摆脱困境的一些动作列表，这里呢也可以使用插件来弄，你sonictl的摆脱性能好不好就看这个核心技术了哟！！![抓狂](http://static.blog.csdn.net/xheditor/xheditor_emot/default/crazy.gif)


~controller\_frequency ( 
double, default: 20.0) 

* The rate in Hz at which to run the control loop and send velocity commands to the base.  
 控制器发布速度命令的频率（看看，默认值20！一般设置为3。这里暗示了一个向底盘发送速度指令的频率要求，而不是给个值就不管了~~）


~planner\_patience ( 
double, default: 5.0) 

* How long the planner will wait in seconds in an attempt to find a valid plan before space-clearing operations are performed.  
 字面意思是：规划器在一个操作执行前等几秒钟就去找合适的路径，那个操作就是 space-clearing 。这个有点confusing
* 理解看的话，应该是 全局/局部 规划器要在 space-clearing 操作（obstacle就会被清掉）执行几秒之后开始规划路径。而不是之前~~![疑问](http://static.blog.csdn.net/xheditor/xheditor_emot/default/doubt.gif)不懂~  
 操作器找路5s后，如果还找不到，就space-clearing操作？


~controller\_patience ( 
double, default: 15.0) 

* How long the controller will wait in seconds without receiving a valid control before space-clearing operations are performed.  
 控制器要等15s没有接到一个合法的控制，然后就要执行space-clearing操作（obstacle就会被清掉）了。


~conservative\_reset\_dist ( 
double, default: 3.0) 

* The distance away from the robot in meters at which obstacles will be cleared from the[costmap](http://wiki.ros.org/costmap_2d) when attempting to clear space in the map. Note, this parameter is only used when the default recovery behaviors are used formove\_base.  
 距离机器人多远的距离的obstacle就会被从costmap上清掉。注意，只有 default recovery behaviors are used for  move\_base 时，这个参数才有用。


~recovery\_behavior\_enabled ( 
bool, default: 
true) 

* Whether or not to enable the  move\_base recovery behaviors to attempt to clear out space.   
 是否允许 move\_base 的恢复动作来clear out space.
* clear out space 究竟是什么意思？刷新costmap上的obstacle？


~clearing\_rotation\_allowed ( 
bool, default: 
true) 

* Determines whether or not the robot will attempt an in-place rotation when attempting to clear out space. Note: This parameter is only used when the default recovery behaviors are in use, meaning the user has not set therecovery\_behaviors parameter to anything custom.   
 要不要机器人在clear out space时执行一个原地转圈。注意，只有 default recovery behaviors are used for  move\_base 时，这个参数才有用。这意味着用户sonictl并没有把 recovery\_behaviors 这个参数设置为 anything custom(定制的任何值)。


~shutdown\_costmaps ( 
bool, default: 
false) 

* Determines whether or not to shutdown the costmaps of the node whenmove\_base is in an inactive state  
 当move\_base在不活动状态时，是不是要关掉move\_base node的 costmap


~oscillation\_timeout ( 
double, default: 0.0) 

* How long in seconds to allow for oscillation before executing recovery behaviors. A value of 0.0 corresponds to an infinite timeout.**New in navigation 1.3.1** 在执行恢复动作前等多久，来 oscillation （晃荡），如果是 0.0 的话，意味着可以永久晃荡下去.sonictl at CSDN。


~oscillation\_distance ( 
double, default: 0.5) 

* How far in meters the robot must move to be considered not to be oscillating. Moving this far resets the timer counting up to the~oscillation\_timeout**New in navigation 1.3.1** 机器人跑多远才不被认为是在晃荡，超过这个值就会重设那个定时器，而定时器的timeout时间是由 ~oscillation\_timeout 参数指定的。


~planner\_frequency ( 
double, default: 0.0) 

* The rate in Hz at which to run the global planning loop. If the frequency is set to 0.0, the global planner will only run when a new goal is received or the local planner reports that its path is blocked.**New in navigation 1.6.0** 频率 - 就是全局规划器循环的频率Hz. 0.0 = 只有当收到一个新的goal时或收到local planner发的path is blocked报告时才进行一次全局规划。sonictl认为这个也很重要！


  


### 【Component APIs】



The move\_base node 包含的这些组件都有它们自己的ROS API, 也许因为这些组件的值而会不同，具体的Links就在下面列出供查询。  
 The move\_base node contains components that have their own ROS APIs. These components may vary based on the values of the~base\_global\_planner,~base\_local\_planner, and~recovery\_behaviors respectively. Links to the APIs for the default components can be found below:


* [costmap\_2d](http://wiki.ros.org/costmap_2d) - Documentation on the[costmap\_2d](http://wiki.ros.org/costmap_2d) package used in move\_base
* [nav\_core](http://wiki.ros.org/nav_core) - Documentation on thenav\_core::BaseGlobalPlanner andnav\_core::BaseLocalPlanner interfaces used bymove\_base.
* [base\_local\_planner](http://wiki.ros.org/base_local_planner) - Documentation on the[base\_local\_planner](http://wiki.ros.org/base_local_planner) used inmove\_base
* [navfn](http://wiki.ros.org/navfn) - Documentation on the[navfn](http://wiki.ros.org/navfn) global planner used inmove\_base
* [clear\_costmap\_recovery](http://wiki.ros.org/clear_costmap_recovery) - Documentation on the[clear\_costmap\_recovery](http://wiki.ros.org/clear_costmap_recovery) recovery behavior used inmove\_base
* [rotate\_recovery](http://wiki.ros.org/rotate_recovery) - Documentation on the[rotate\_recovery](http://wiki.ros.org/rotate_recovery) recovery behavior used inmove\_base

 Class Diagram (partially & not strictly drawn) is available 
 [here](https://docs.google.com/drawings/d/1rKY4yxI0ibHqgQoLFZPuEayRp8Bs4FpRyaNKsMxqpX4/edit?usp=sharing). 
  
 


======== 以上就是我sonictl的读书笔记 ======  
 


  
 


  
 


  
 


  
 


  
 


  
 





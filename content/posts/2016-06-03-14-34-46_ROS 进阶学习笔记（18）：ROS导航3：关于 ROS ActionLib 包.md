---
layout: post
title:  "ROS 进阶学习笔记（18）：ROS导航3：关于 ROS ActionLib 包"
date: 2016-06-03 14:34:46 +0800
tags: 
slug: p20160603143446
---

# ROS 进阶学习笔记（18）：ROS导航3：关于 ROS ActionLib 包





## ROS 进阶学习笔记（18）：理解ROS Action Lib


ROS 里除了基本的msg publisher/Subscriber 以外，还有以下东东需要理解：  
 


* Server/Client - 参见[Link](http://blog.csdn.net/sonictl/article/details/51372534#t3)
* ActionLib - 本次blog讨论


ActionLib是ROS里一个重要的部分（link:http://wiki.ros.org/APIs），既然我在学习move\_base时遇到很多有关actionlib的实现，还是先把actionlib理解之后再回去搞navigation的其他包比较合适。打好基础。


![](https://img-blog.csdn.net/20160602210846795?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 [查看原图](https://img-blog.csdn.net/20160602210846795?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


### [Overview]


ROS中的服务service是一问一答的形式，你来查询了，我就返给你要的信息。  
 action也有服务的概念，但是它不一样的地方是：不是一问一答，而多了一个反馈，它会不断反馈项目进度。如navigation下的move\_base package，你设定了目标点，反馈信息可能是机器人在规划路径上的即时位姿，直到机器人到达目标点，返回SUCCEEDED消息。有关actionlib的具体学习笔记sonictl见下：


### === 我把wiki\_actionlib的读书笔记写在下面： ===


wikipage: <http://wiki.ros.org/actionlib>  
 


### 【Summary】


The actionlib stack provides a standardized interface for interfacing with preemptable tasks. Examples of this include moving the base to a target location, performing a laser scan and returning the resulting point cloud, detecting the handle of a door, etc.


actionlib是个stack，意味着与navigation stack平级？为和pre emptable tasks 可预先中止性任务进行交互而提供标准的接口。相关的例子有：moving base to a target location，performing a laser scan and returning the resulting point cloud, detecting the handle of a door, etc.  
 （sonictl@csdn:看起来还是很必须的一块内容，如果希望往高级一点的ROS应用发力的话。)  
   
   
 


### 1. 【Overview】


In any large ROS based system, there are cases when someone would like to send a request to a node to perform some task, and also receive a reply to the request. This can currently be achieved via ROS services.  
   
 In some cases, however, if the service takes a long time to execute, the user might want the ability to cancel the request during execution or get periodic feedback about how the request is progressing. The actionlib package provides tools to create servers that execute long-running goals that can be preempted. It also provides a client interface in order to send requests to the server.  
   
 有的大点的ROS系统项目，有些时候需要向一个节点发送一个request然后等它处理完了发回一个response.这个已经可以通过ROS里的Service的概念达到了。但有时候呢，如果这个service需要稍长一些的时间来运算之后才能反馈所需的结果，但在运行过程中，用户sonictl也许要根据运行的不同情况来决定是否取消request或者是需要周期性地获知运行状态。The actionlib 包就是提供这些工具，使得可以创建一些servers, 这些servers是用来执行需要长时间的任务的，并且是可以被preempted（预先制止）的。同时，actionlib包还提供了一个client接口，用来给server发请求。（sonictl@csdn\_blog：这使我想到了move\_base包可以被 cancle\_goal 的功能）  
 


### 2. 【Detailed Description】


For a full discussion of how actionlib operates "under the hood", please see the[Detailed Description](http://wiki.ros.org/actionlib/DetailedDescription).  
 这里居然不就actionlib的实现机制进一步展开，而是让读者自己去看。我进去看了一下，感觉要ROS上上一台阶，应该好好理解它里面的机制。  
 


### 3. 【Client-Server Interaction】


  \*~~由于我有更重要的事情需要解决，暂时actionlib笔记就到这里等~~我回头来继续![微笑](http://static.blog.csdn.net/xheditor/xheditor_emot/default/smile.gif)  --- sonictl@csdn\_blog  
 The *ActionClient* and *ActionServer* communicate via a *"ROS Action Protocol"*, which is built on top of ROS messages. The client and server then provide a simple API for users to request goals (on the client side) or to execute goals (on the server side) via function calls and callbacks.   
     上面所述的 ActionClient 和 ActionServer 通过 ROS Action Protocol （ROS Action 协议，建立在ROS messages的基础上）来通信。Client&Server 为用户提供了简单的API，通过函数的调用和回调，用来在client端request一个目标，或者，在server端来执行达成一个目标。下图说明这个机制如何运行：  
 ![](https://img-blog.csdn.net/20160801191535135?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 这个其实是Client占主动，ActionServer占被动那意思~--- sonictl, csdn\_blog



### 4. 【Action Specification: Goal, Feedback, & Result】



In order for the client and server to communicate, we need to define a few messages on which they communicate. This is with an*action specification*. This defines the Goal, Feedback, and Result messages with which clients and servers communicate:  
 首先是如何定义messages来让Client&Server之间通信，这就是所谓的 action specification (Action说明书-的概念) 。它定义了Goal,Feedback,and Result messages来让Client和Server通信：


**4.1 Goal**  
 To accomplish tasks using actions, we introduce the notion of a goal that can be sent to an ActionServer by an ActionClient. In the case of moving the base, the goal would be a PoseStamped message that contains information about where the robot should move to in the world. For controlling the tilting laser scanner, the goal would contain the scan parameters (min angle, max angle, speed, etc).  
 为了用actions机制完成一个任务，我们引入了the notion of a goal, 这个goal可以被ActionClient发送到ActionServer. 比如在move base这个案例中，它的类型是PoseStamped，包含了机器人应该到达的哪里的信息。在激光扫描云台控制案例中，the goal会包含扫描的参数（min angle, max angle, speed 等）


**Feedback**  
 Feedback provides server implementers a way to tell an ActionClient about the incremental progress of a goal. For moving the base, this might be the robot's current pose along the path. For controlling the tilting laser scanner, this might be the time left until the scan completes.   
 Feedback是server用来告诉ActionClient goal执行过程中的各种情况。在moving\_base案例中，它是机器人现在的位姿；在controlling the tilting laser scanner案例中, this might be the time left until the scan completes（扫描剩余时间）.  
 


**Result**  
 A result is sent from the ActionServer to the ActionClient upon completion of the goal. This is different than feedback, since it is sent exactly once. This is extremely useful when the purpose of the action is to provide some sort of information. For move base, the result isn't very important, but it might contain the final pose of the robot. For controlling the tilting laser scanner, the result might contain a point cloud generated from the requested scan.  
 执行结果，比如在move\_base中的结果和机器人pose；在云台激光扫描中的一个请求的点云数据。等



### 5.【.action File】



The action specification is defined using a  .action file. The .action file has the goal definition, followed by the result definition, followed by the feedback definition, with each section separated by 3 hyphens (**---**).  
 熟悉Message机制的朋友就会知道，这个跟那个类似，就是定义 action specification的消息数据类型滴~  -- sonictl\_at\_blog\_csdn\_net  
 


These files are placed in a package's ./action directory, and look extremely similar to a service's.srv file. An action specification for doing the dishes might look like the following:


./action/DoDishes.action  



```
# Define the goal
uint32 dishwasher_id  # Specify which dishwasher we want to use
---
# Define the result
uint32 total_dishes_cleaned
---
# Define a feedback message
float32 percent_complete
```



Based on this .action file, 6 messages(y? 可能是Client&Server各产生3个？) need to be generated in order for the client and server to communicate. This generation can be automatically triggered during the make process:



#### Catkin



Add the following to your CMakeLists.txt file *before*  catkin\_package(). 注意你要用 actionlib 的话，记得修改CMakeLists.txt 文件！




```
find_package(catkin REQUIRED genmsg actionlib_msgs actionlib)
add_action_files(DIRECTORY action FILES DoDishes.action)
generate_messages(DEPENDENCIES actionlib_msgs)
```



Additionally, the package's package.xml must include the following dependencies:



```
<build_depend>actionlib</build_depend>
<build_depend>actionlib_msgs</build_depend>
<run_depend>actionlib</run_depend>
<run_depend>actionlib_msgs</run_depend>
```




#### Rosbuild



If you are using rosbuild instead of catkin, instead add the following *before* rosbuild\_init().




```
rosbuild_find_ros_package(actionlib_msgs)
include(${actionlib_msgs_PACKAGE_PATH}/cmake/actionbuild.cmake)
genaction()
```



Then, *after* the output paths, uncomment (or add)  




```
rosbuild_genmsg()
```



*Note:* rosbuild\_genmsg() must be called after the output paths have been set.


For 1.0 series (i.e. boxturtle) use:  



```
rosbuild_find_ros_package(actionlib)
include(${actionlib_PACKAGE_PATH}/cmake/actionbuild.cmake)
genaction()
rosbuild_genmsg()
```



Additionally, the package's manifest.xml must include the following dependencies:



```
<depend package="actionlib"/>
<depend package="actionlib_msgs"/>
```




#### Results



For the DoDishes.action, the following messages are generated by genaction.py:


* DoDishesAction.msg
* DoDishesActionGoal.msg
* DoDishesActionResult.msg
* DoDishesActionFeedback.msg
* DoDishesGoal.msg
* DoDishesResult.msg
* DoDishesFeedback.msg


These messages are then used internally by actionlib to communicate between the ActionClient and ActionServer.



### 6. 【Using the ActionClient】





#### C++ SimpleActionClient



Full API Reference for the  [C++ SimpleActionClient](http://www.ros.org/doc/api/actionlib/html/classactionlib_1_1SimpleActionClient.html) 


**Quickstart Guide:**   
 Suppose you have defined  DoDishes.action in the chores package. The following snippet shows how to send a goal to a DoDishes ActionServer called "do\_dishes".




[切换行号显示](http://wiki.ros.org/actionlib#)

```
 [1](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_1) #include <chores/DoDishesAction.h>
 [2](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_2) #include <actionlib/client/simple_action_client.h>
 [3](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_3) 
 [4](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_4) typedef actionlib::SimpleActionClient<chores::DoDishesAction> Client;
 [5](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_5) 
 [6](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_6) int main(int argc, char** argv)
 [7](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_7) {
 [8](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_8)   ros::init(argc, argv, "do_dishes_client");
 [9](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_9)   Client client("do_dishes", true); // true -> don't need ros::spin()
 [10](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_10)   client.waitForServer();
 [11](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_11)   chores::DoDishesGoal goal;
 [12](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_12)   // Fill in goal here
 [13](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_13)   client.sendGoal(goal);
 [14](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_14)   client.waitForResult(ros::Duration(5.0));
 [15](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_15)   if (client.getState() == actionlib::SimpleClientGoalState::SUCCEEDED)
 [16](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_16)     printf("Yay! The dishes are now clean");
 [17](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_17)   printf("Current State: %s\n", client.getState().toString().c_str());
 [18](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_18)   return 0;
 [19](http://wiki.ros.org/actionlib#CA-9dc13d410514bbb427a5931ec9936896b8cf0abf_19) }

```





**Note:** For the C++ SimpleActionClient, thewaitForServer method will only work if a separate thread is servicing the client's callback queue. This requires passing intrue for the spin\_thread option of the client's constructor, running with a multi-threaded spinner, or using your own thread to service ROS callback queues.



#### Python SimpleActionClient




Full API reference for the  [Python SimpleActionClient](http://www.ros.org/doc/api/actionlib/html/classactionlib_1_1simple__action__client_1_1SimpleActionClient.html) 


Suppose the DoDishes.action exists in thechores package. The following snippet shows how to send a goal to a DoDishes ActionServer called "do\_dishes" using Python.





[切换行号显示](http://wiki.ros.org/actionlib#)

```
 [1](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_1) #! /usr/bin/env python
 [2](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_2) 
 [3](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_3) import roslib
 [4](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_4) roslib.load_manifest('my_pkg_name')
 [5](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_5) import rospy
 [6](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_6) import actionlib
 [7](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_7) 
 [8](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_8) from chores.msg import DoDishesAction, DoDishesGoal
 [9](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_9) 
 [10](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_10) if __name__ == '__main__':
 [11](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_11)     rospy.init_node('do_dishes_client')
 [12](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_12)     client = actionlib.SimpleActionClient('do_dishes', DoDishesAction)
 [13](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_13)     client.wait_for_server()
 [14](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_14) 
 [15](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_15)     goal = DoDishesGoal()
 [16](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_16)     # Fill in the goal here
 [17](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_17)     client.send_goal(goal)
 [18](http://wiki.ros.org/actionlib#CA-01a90787f036b7f609402261d9a26d106ea379bb_18)     client.wait_for_result(rospy.Duration.from_sec(5.0))

```







### Implementing an ActionServer





#### C++ SimpleActionServer



Full API Reference for the  [C++ SimpleActionServer](http://www.ros.org/doc/api/actionlib/html/classactionlib_1_1SimpleActionServer.html) 


**Quickstart Guide:**   
 Suppose you have defined  DoDishes.action in the chores package. The following snippet shows how to write a DoDishes ActionServer called "do\_dishes".




[切换行号显示](http://wiki.ros.org/actionlib#)

```
 [1](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_1) #include <chores/DoDishesAction.h>
 [2](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_2) #include <actionlib/server/simple_action_server.h>
 [3](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_3) 
 [4](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_4) typedef actionlib::SimpleActionServer<chores::DoDishesAction> Server;
 [5](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_5) 
 [6](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_6) void execute(const chores::DoDishesGoalConstPtr& goal, Server* as)
 [7](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_7) {
 [8](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_8)   // Do lots of awesome groundbreaking robot stuff here
 [9](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_9)   as->setSucceeded();
 [10](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_10) }
 [11](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_11) 
 [12](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_12) int main(int argc, char** argv)
 [13](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_13) {
 [14](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_14)   ros::init(argc, argv, "do_dishes_server");
 [15](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_15)   ros::NodeHandle n;
 [16](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_16)   Server server(n, "do_dishes", boost::bind(&execute, _1, &server), false);
 [17](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_17)   server.start();
 [18](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_18)   ros::spin();
 [19](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_19)   return 0;
 [20](http://wiki.ros.org/actionlib#CA-56b4d6a4e75200d0982628494a440756c025c5ba_20) }

```






#### Python SimpleActionServer



Full API Reference for the  [Python SimpleActionServer](http://www.ros.org/doc/api/actionlib/html/classactionlib_1_1simple__action__server_1_1SimpleActionServer.html) 


**Quickstart Guide:**   
 Suppose you have defined  DoDishes.action in the chores package. The following snippet shows how to write a DoDishes ActionServer called "do\_dishes".





[切换行号显示](http://wiki.ros.org/actionlib#)

```
 [1](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_1) #! /usr/bin/env python
 [2](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_2) 
 [3](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_3) import roslib
 [4](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_4) roslib.load_manifest('my_pkg_name')
 [5](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_5) import rospy
 [6](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_6) import actionlib
 [7](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_7) 
 [8](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_8) from chores.msg import DoDishesAction, DoDishesServer
 [9](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_9) 
 [10](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_10) class DoDishesServer:
 [11](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_11)   def __init__(self):
 [12](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_12)     self.server = actionlib.SimpleActionServer('do_dishes', DoDishesAction, self.execute, False)
 [13](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_13)     self.server.start()
 [14](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_14) 
 [15](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_15)   def execute(self, goal):
 [16](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_16)     # Do lots of awesome groundbreaking robot stuff here
 [17](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_17)     self.server.set_succeeded()
 [18](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_18) 
 [19](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_19) 
 [20](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_20) if __name__ == '__main__':
 [21](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_21)   rospy.init_node('do_dishes_server')
 [22](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_22)   server = DoDishesServer()
 [23](http://wiki.ros.org/actionlib#CA-4baf411fac55cd4cf3804a24aff12123a8a053ac_23)   rospy.spin()

```






### 7. 【SimpleActionServer Goal Policies】



The SimpleActionServer implements a single goal policy on top of theActionServer class. The specification of the policy is as follows:


* Only one goal can have an active status at a time
* New goals preempt previous goals based on the stamp in their GoalID field (later goals preempt earlier ones)
* An explicit preempt goal preempts all goals with timestamps that are less than or equal to the stamp associated with the preempt
* Accepting a new goal implies successful preemption of any old goal and the status of the old goal will be change automatically to reflect this


Calling acceptNewGoal accepts a new goal when one is available. The status of this goal is set to active upon acceptance, and the status of any previouslyactive goal is set to preempted. Preempts received for the new goal betweenchecking if isNewGoalAvailable or invocation of a goal callback and theacceptNewGoal call will not trigger a preempt callback. This means,isPreemptRequested should be called after accepting the goal even forcallback-based implementations to make sure the new goal does not have apending preempt request.



### 8. 【Tutorials】



Please refer to the  [Tutorials](http://wiki.ros.org/actionlib/Tutorials) page 



### Report a Bug



Use trac to  [report bugs](https://code.ros.org/trac/ros-pkg/newticket?component=common&type=defect&&common) or  [request features](https://code.ros.org/trac/ros-pkg/newticket?component=common&type=enhancement&common). [[View active tickets](https://code.ros.org/trac/ros-pkg/query?component=common&status=assigned&status=new&status=reopened)]  
 



### Code Quality

 A set of Code Metrics representing the quality of the code. The metrics are classified into file-, function-, and class-metrics. Only C++ files are considered during analysis. Information about the code quantity are provided in section Lines of Code. For more information please visit the 
 [code quality main page](http://wiki.ros.org/code_quality). 
* Status:  [job status](http://jenkins.willowgarage.com:8080/)
* Date & Time of Analysis: 2013-08-24 10:03:00
* Analyzed code: <https://github.com/ros-gbp/actionlib-release.git>
* Analysis tool: QA-C++ provided by Programming Research Ltd.


#### Lines of Code




| **Type** | **# of Files** | **# of Code Lines** | **# of Comment Lines** |
| --- | --- | --- | --- |
| C++ | 14 | 769 | 512 |
| C/C++ Header | 33 | 3212 | 1941 |
| CMake | 3 | 115 | 8 |
| Python | 24 | 1985 | 1163 |
| XML | 1 | 30 | 0 |
| **SUM** | **75** | **6111** | **3624** |


#### Code Metrics


##### File-based metrics


###### Comment to code ratio


This metric is defined to be the number of visible characters in comments divided by the number of visible characters outside comments. Comment delimiters are ignored. Whitespace characters in strings are treated as visible characters. A large metric value may indicate that there are too many comments - an attribute that can make a module difficult to read. A small value may indicate that there are not enough.



**Info** 


* Distro average:  
 0.86
* Stack average:  
 1.17
* [Thresholds](http://wiki.ros.org/code_quality#Thresholds):    
 0.2 < x < 2.0
* In range:  
 100.0 %



##### Function-based metrics


###### Maximum nesting of control structures


This metric is a measure of the maximum control flow nesting in your source code. You can reduce the value of this metric by turning your nesting into separate functions. This will improve the readability of the code by reducing both the nesting and the average cyclomatic complexity per function.



**Info** 


* Distro average:  
 1.01
* Stack average:  
 2.21
* [Thresholds](http://wiki.ros.org/code_quality#Thresholds):    
 0 < x < 5
* In range:  
 100.0 %



###### Number of executable lines


The count of lines of a function body (starting at the first non-comment token that follows the exception specification, through to the last brace) that contain tokens, where lines that only contain any of the following are not counted: {, }, comment and all tokens of declarations.



**Info** 


* Distro average:  
 10.63
* Stack average:  
 8.21
* [Thresholds](http://wiki.ros.org/code_quality#Thresholds):    
 1 < x < 70
* In range:  
 28.6 %



###### Cyclomatic complexity


Cyclomatic complexity is calculated as the number of decisions plus 1. High cyclomatic complexity indicates inadequate modularization or too much logic in one function. Software metric research has indicated that functions with a cyclomatic complexity greater than 10 tend to have problems related to their complexity.



**Info** 


* Distro average:  
 4.63
* Stack average:  
 12.14
* [Thresholds](http://wiki.ros.org/code_quality#Thresholds):    
 1 < x < 15
* In range:  
 64.3 %



###### Number of Function Calls


The number of function calls within a function. Functions with a large number of function calls are more difficult to understand because their functionality is spread across several components. Note that the calculation of STSUB is based on the number of function calls and not the number of distinct functions that are called.



**Info** 


* Distro average:  
 17.05
* Stack average:  
 32.64
* [Thresholds](http://wiki.ros.org/code_quality#Thresholds):    
 1 < x < 10
* In range:  
 21.4 %



###### Estimated static path count


This gives an upper bound on the number of possible paths in the control flow of the function. It is the number of non-cyclic execution paths in a function.



**Info** 


* Distro average:  
 10265104.76
* Stack average:  
 35714665.71
* [Thresholds](http://wiki.ros.org/code_quality#Thresholds):    
 1 < x < 250
* In range:  
 64.3 %



##### Class-based metrics


###### Number of methods available in class


The number of methods declared within a class. This does not include methods declared in base classes. Classes with a large number of methods will be difficult to understand.



**Info** 


* Distro average:  
 6.58
* Stack average:  
 7.0
* [Thresholds](http://wiki.ros.org/code_quality#Thresholds):    
 1 < x < 20
* In range:  
 100.0 %



###### Deepest level of inheritance


This represents the number of derivations from the furthest base class down to this class. A high figure may indicate that the class depends on accumulated functionality, which makes understanding the class potentially difficult. This is one of the metrics defined by Chidamber & Kemerer.

 <div style="width:190px; height:100px; > 
**Info** 


* Distro average:  
 0.36
* Stack average:  
 0.0
* [Thresholds](http://wiki.ros.org/code_quality#Thresholds):    
 1 < x < 5
* In range:  
 0.0 %


  

  

  

  
 


  
 


  
 


  
 





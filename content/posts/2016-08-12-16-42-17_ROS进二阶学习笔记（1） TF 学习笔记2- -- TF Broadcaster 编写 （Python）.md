---
layout: post
title:  "ROS进二阶学习笔记（1） TF 学习笔记2- -- TF Broadcaster 编写 （Python）"
date: 2016-08-12 16:42:17 +0800
tags: 
slug: p20160812164217
---

# ROS进二阶学习笔记（1） TF 学习笔记2- -- TF Broadcaster 编写 （Python）





## ROS进二阶学习笔记（1） TF 学习笔记2 -- TF Broadcaster


Ref:  <http://wiki.ros.org/tf/Tutorials#Learning_tf>  
 >>Ref:  [Writing a tf broadcaster (Python)](http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20broadcaster%20%28Python%29)


This tutorial teaches you how to broadcast the state of a robot to tf. 


Ref内的东西请读者自行研读，这里要做点总结：


  
 


1. /tf topic上 是有一个发布器broadcaster 发布 /tf 消息才能被 /tf 的 listener 收听到。
2. /tf 的发布器类：python: tf.TransformBroadcaster 所以我们要理解它里面的方法： sendTransform(translation, rotation, time, child, parent)


  

### 1. 先看看tf broadcaster的示例代码：




```
1. #!/usr/bin/env python
2. import roslib
3. roslib.load_manifest('learning_tf')
4. import rospy
5. 
6. import tf
7. import turtlesim.msg
8. 
9. def handle_turtle_pose(msg, turtlename):
10. br = tf.TransformBroadcaster()
11. br.sendTransform((msg.x, msg.y, 0), #the translation of the transformtion as a tuple (x, y, z)
12. tf.transformations.quaternion_from_euler(0, 0, msg.theta),
13. #the rotation of the transformation as a tuple (x, y, z, w)
14. rospy.Time.now(), #the time of the transformation, as a rospy.Time()
15. turtlename, #child frame in tf, string
16. "world") #parent frame in tf, string
17. 
18. if __name__ == '__main__':
19. rospy.init_node('turtle_tf_broadcaster')
20. turtlename = rospy.get_param('~turtle')
21. #takes parameter "turtle", which specifies a turtle name, e.g. "turtle1" or "turtle2"
22. rospy.Subscriber('/%s/pose' % turtlename, # subscribe to turtlename's /pose topic
23. turtlesim.msg.Pose,      # Pose message data structure
24. handle_turtle_pose,      # callback function,
25. turtlename)              # argument for callback function
26. rospy.spin()
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```


#### 上面代码的框架就是：


1. 通过参数知道turtle是1号还是2号，比如： turtle1。 赋值给turtlename变量
2. 收听 turtle1/pose主题，上面是turtle1的位姿数据。
3. 收听到主题上有message就调用 handle\_turtle\_pose这个函数。后面跟了一个turtlename参数，正好对应def handle\_turtle\_pose()函数定义时的第2个参数：turtlename


#### 补充一点：


这里要补充一点python回调函数调用时，传参的知识：




```
1. #define the callback method:
2. def callback(arg1, arg2, arg3):
3. ...
4. 
5. #next, Subscriber:
6. rospy.Subscriber(topicName, data_type, callback, arg2)

```

上面callback 的 arg1 一般是subscriber读到的数据，直接作为第一个参数传入callback函数。arg2传入callback的arg2位置。
  


### 2. 重点是TransformBroadcaster() 类


参考：<http://mirror.umd.edu/roswiki/doc/diamondback/api/tf/html/python/tf_python.html#transformbroadcaster>  
 


![](https://img-blog.csdn.net/20160905163551194?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


读一读上面的定义：


* translation - 元素组形式的，变换的坐标的转换。tuple(元素组)![睡觉](http://static.blog.csdn.net/xheditor/xheditor_emot/default/sleep.gif) 据我理解，就是tf的位置变换（x，y，z）形式。
* rotation - 四元数数据结构，元素组形式的变换，的旋转。方向变换，上面实例调用了欧拉旋转（Roll, Pitch, Yaw）, 比如有人这样用：  
 quat = tf.transformations.quaternion\_from\_euler(roll, pitch, yaw)
* time - rospy.Time() 数据结构的时间，变换时的时间
* child - 子坐标系; 这里，我们的子坐标系是：turtlename = "turtle1";
* parent - 父坐标系 ;  父坐标系是“world”.


再来看这个源代码：




```
1. br.sendTransform((msg.x, msg.y, 0), #the translation of the transformtion as a tuple (x, y, z)
2. tf.transformations.quaternion_from_euler(0, 0, msg.theta),
3. #the rotation of the transformation as a tuple (x, y, z, w)
4. rospy.Time.now(), #the time of the transformation, as a rospy.Time()
5. turtlename, #child frame in tf, string
6. "world") #parent frame in tf, string

```

  
 就比较清楚了，原料是 msg.x; msg.y; msg.theta，要完成坐标变换的发布： 

1. 平移变换，用了msg.x 、 msg.y
2. 旋转变换，调了 tf.transformations.quaternion\_from\_euler函数，将（0,0，msg.theta）转换成了quaternion四元数。


=========


### 总结


至此，我们就解读清楚了 tf 发布器的编写方法。  
 发布器只是把 child\_frame 与 parent\_frame之间的平移和旋转关系发布在 /tf 主题上，TF\_Listener怎么解读和利用，是更重要的。  
 下一节我们将看看如何写TF\_Listener来完成简单的任务。


  
 


### 通过launch 文件启动这个TF Broadcaster


要测试，我们还得借助ROS提供的turtlesim工具，看看launch文件：




```
1. <launch>
2. <!-- Turtlesim Node-->
3. <node pkg="turtlesim" type="turtlesim_node" name="sim"/>
4. <node pkg="turtlesim" type="turtle_teleop_key" name="teleop" output="screen"/>
5. 
6. <node name="turtle1_tf_broadcaster" pkg="learning_tf" type="turtle_tf_broadcaster.py" respawn="false" output="screen" >
7. <param name="turtle" type="string" value="turtle1" />
8. </node>
9. <node name="turtle2_tf_broadcaster" pkg="learning_tf" type="turtle_tf_broadcaster.py" respawn="false" output="screen" >
10. <param name="turtle" type="string" value="turtle2" />
11. </node>
12. 
13. </launch>

```

 做个简单解读： 
  

1. turtlesim/turtlesim\_node启动，名：sim -- 这是打开带海龟的窗口。这个turtlesim\_node节点会publish 一个turtleX/pose 主题，会被我们turtle\_tf\_broadcaster接收用来发布tf信息。
2. turtlesim/turtle\_teleop\_key , 名：teleop -- 这是启动接收键盘指令的节点。
3. pkg="learning\_tf" type="turtle\_tf\_broadcaster.py" , 启动一个TF Broadcaster, 发布 turtle1 相对于world的tf信息。
4. pkg="learning\_tf" type="turtle\_tf\_broadcaster.py" , 启动一个TF Broadcaster, 发布 turtle2 相对于world的tf信息。



### Checking the results




Now, use the **tf\_echo** tool to check if the turtle pose is actually getting broadcast to tf:




```
 $ rosrun tf tf_echo /world /turtle1
```

This should show you the pose of the first turtle. Drive around the turtle using the arrow keys (make sure your terminal window is active, not your simulator window). If you run 
tf\_echo for the transform between the world and turtle 2, you should not see a transform, because the second turtle is not there yet. However, as soon as we add the second turtle in the next tutorial, the pose of turtle 2 will be broadcast to tf. 
  

  
 


  
 


  
 





文章知识点与官方知识档案匹配，可进一步学习相关知识[Python入门技能树](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)[首页](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)[概览](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)422843 人正在系统学习中

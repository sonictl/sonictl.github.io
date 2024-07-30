---
layout: post
title:  "ROS进二阶学习笔记（1） TF 学习笔记3 -- TF Listener 编写 （Python）and Adding frame(Python)"
date: 2016-09-05 17:37:15 +0800
tags: 
slug: p20160905173715
---

# ROS进二阶学习笔记（1） TF 学习笔记3 -- TF Listener 编写 （Python）and Adding frame(Python)





## ROS进二阶学习笔记（1） TF 学习笔记3 -- TF Listener 编写 （Python）


Ref: <http://wiki.ros.org/tf/Tutorials/Writing%20a%20tf%20listener%20%28Python%29>


In the previous tutorials we created a tf broadcaster to publish the pose of a turtle to tf. In this tutorial we'll create a tf listener to start using tf. and use tf to get access to [frame](https://so.csdn.net/so/search?q=frame&spm=1001.2101.3001.7020) transformations.  
 还是要请先读了Ref 再来这里sonictl@csdn看总结：


* 这个listen的程序做了3件事情：
* 1. 放个 turtle2乌龟到舞台上去，用的turtlesim/Spawn服务；
* 2. 用tf.TransformListener类下的lookupTransform方法，查询turtle2 相对于 turtle1 的位置。
* 3. 用查到的相对位置来发布cmd速度和角速度信息，引导 turtle2 运动


  
 


### 看看代码


我小改了下代码： (path: nodes/learning\_tf\_listener.py )  
 




```
1. #!/usr/bin/env python
2. import roslib
3. roslib.load_manifest('learning_tf')
4. import rospy
5. import math
6. import tf
7. import geometry_msgs.msg
8. import turtlesim.srv
9. 
10. if __name__ == '__main__':
11. rospy.init_node('tf_turtle')
12. 
13. #next three lines: Create another turtle onto the stage
14. rospy.wait_for_service('spawn')
15. spawner = rospy.ServiceProxy('spawn', turtlesim.srv.Spawn)
16. #turtlesim has a service named: Spawn
17. spawner(4, 4, 0, 'turtle2')  #(x,y,theta,name)
18. 
19. #Publisher for the turtle2's movement
20. turtle_vel = rospy.Publisher('turtle2/cmd_vel', geometry_msgs.msg.Twist,queue_size=1)
21. 
22. listener = tf.TransformListener() #tf transforListener
23. 
24. rate = rospy.Rate(10.0)
25. while not rospy.is_shutdown():
26. #All this is wrapped in a try-except block to catch possible exceptions:
27. try:
28. (trans,rot) = listener.lookupTransform('/turtle2', '/turtle1', rospy.Time(0))
29. except (tf.LookupException, tf.ConnectivityException, tf.ExtrapolationException):
30. continue
31. 
32. angular = 4 * math.atan2(trans[1], trans[0])
33. linear = 0.5 * math.sqrt(trans[0] ** 2 + trans[1] ** 2)
34. cmd = geometry_msgs.msg.Twist() #declare an Twist object
35. cmd.linear.x = linear
36. cmd.angular.z = angular
37. turtle_vel.publish(cmd)  # publish the cmd out
38. 
39. rate.sleep()
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  


### 总结一下：


    tf要解决的问题是: 当 A相对B的tf - A\_B , C相对B的tf - C\_B都已经被发布到 tf 上时，如何查询 A\_C or C\_A 的tf的问题。


1. 通过/tf 的发布器类：python: tf.TransformBroadcaster 的方法： sendTransform(translation, rotation, time, child, parent) 发布tf
2. 用tf.TransformListener类下的lookupTransform方法，查询 C 相对于 A 的位置
3. 抛出一个问题： frame name是在哪里指定的？ 比如，我们加一个laser\_scanner，它的frame name要怎么配置才能让lookupTransform方法看得懂？
4. rospy.Time(0) 这是什么含义? 跟rospy.Time.now() 有啥区别？


  
 


== 接下来做点测试 ：   
 



### Running the listener





With your text editor, open the launch file called **start\_demo.launch**, and add the following lines:



```
  <launch>
    ...
    <node pkg="learning_tf" type="turtle_tf_listener.py" 
          name="listener" />
  </launch>
```






First, make sure you stopped the launch file from the previous tutorial (use ctrl-c). Now you're ready to start your full turtle demo:




```
 $ roslaunch learning_tf start_demo.launch
```


You should see the turtlesim with **two** turtles.



### Checking the results



To see if things work, simply drive around the first turtle using the arrow keys (make sure your terminal window is active, not your simulator window), and you'll see the second turtle following the first one!


When the turtlesim starts up you may see:  


* ```
[ERROR] 1253915565.300572000: Frame id /turtle2 does not exist! When trying to transform between /turtle1 and /turtle2.
[ERROR] 1253915565.401172000: Frame id /turtle2 does not exist! When trying to transform between /turtle1 and /turtle2.
```


This happens because our listener is trying to compute the transform before messages about turtle 2 have been received because it takes a little time to spawn in turtlesim and start broadcasting a tf frame.



## ROS TF 学习笔记 -- Adding a frame （Python）



Ref:  [Adding a frame (Python)](http://wiki.ros.org/tf/Tutorials/Adding%20a%20frame%20%28Python%29) -- This tutorial teaches you how to add an extra fixed frame to tf.  
 


先看Ref, 再来总结：


* 理解add frame的意义和在哪里加frame
* a frame only has one single parent, but it can have multiple children
* 实质还是一个tf broadcaster。


### 看看代码：


Fire up your favorite editor and paste the following code into a new file callednodes/fixed\_tf\_broadcaster.py



```
1. #!/usr/bin/env python
2. import roslib
3. roslib.load_manifest('learning_tf')
4. 
5. import rospy
6. import tf
7. 
8. if __name__ == '__main__':
9. rospy.init_node('my_tf_broadcaster')
10. br = tf.TransformBroadcaster()
11. rate = rospy.Rate(10.0)
12. while not rospy.is_shutdown():
13. br.sendTransform((0.0, 2.0, 0.0), #The carrot1 frame is 2 meters offset from the turtle1 frame
14. (0.0, 0.0, 0.0, 1.0), #no rotation
15. rospy.Time.now(),
16. "carrot1",
17. "turtle1")
18. rate.sleep()
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  
 后面有个Broadcasting a moving frame的例程，就不贴了，自己看Ref 
  

  
 


  

  
 




文章知识点与官方知识档案匹配，可进一步学习相关知识[Python入门技能树](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)[首页](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)[概览](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)422843 人正在系统学习中

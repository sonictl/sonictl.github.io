---
layout: post
title:  "ROS进二阶学习笔记（8）-- 关于rospy 和 parameters"
date: 2017-01-11 17:53:10 +0800
tags: 
slug: p20170111175310
---

# ROS进二阶学习笔记（8）-- 关于rospy 和 parameters





ref:


[http://wiki.ros.org/Parameter Server](http://wiki.ros.org/Parameter%20Server)  --  总领阐述parameter的一些概念。比如namespace  
 [http://wiki.ros.org/rospy/Overview/Parameter Server](http://wiki.ros.org/rospy/Overview/Parameter%20Server)  --  如何使用Python操作[params](https://so.csdn.net/so/search?q=params&spm=1001.2101.3001.7020)  
 <http://wiki.ros.org/rospy_tutorials/Tutorials/Parameters>  --  如何使用Python操作params<http://wiki.ros.org/roslaunch/XML> -- 使用launchfile操作 param<http://wiki.ros.org/roslaunch/XML/param> -- 使用launchfile 操作param<http://wiki.ros.org/rosparam>  --  ROS Tool之rosparam用法  
 


  
 


## ros进阶学习笔记（24）-- 关于rospy 和 parameters


进行rospy编程时，需要用到parameter，但如何灵活用好它，还需进一步学习总结。


* 节点程序如何声明parameter，是global, relative or private?
* 声明完了如何通过外部为rosparam赋值？rosrun命令行怎么赋？launch文件怎么赋？yaml怎么赋？
* 


### 参数Parameters - 概念


  
 


    
 


### 一个简单的 python 代码 - 使用params


  
 


  
 


### rosparam工具


  
 


  
 


### arguments 赋值 mynode.py


   http://answers.ros.org/question/64552/how-to-pass-arguments-to-python-node/  
 




```
1. #!/usr/bin/env python
2. import sys
3. class MyNodeClass():
4. #Setup the node:
5. def __init__(self):
6. ...
7. rospy.set_param('~n_naviLoop', sys.argv[1])
8. rospy.set_param('~layoverSecs', sys.argv[2])
9. print rospy.get_param("~n_naviLoop")
10. print rospy.get_param("~layoverSecs")
11. 
12. if __name__ == '__main__':
13. if len(sys.argv)<3 :
14. print "Usage: mynode.py arg1 arg2"
15. else:
16. try:
17. MyNodeClass()
18. rospy.spin()
19. except rospy.ROSInterruptException:
20. rospy.loginfo("my_node finished.")
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

命令行赋值上面代码中的 n\_naviLoop:   
        $ rosrun mypkg my\_node.py 3 5  //n\_naviLoop = 3; layoverSecs = 5;


#### 另一例：


    Launch File + Arguments + CommandLine 赋值方式：


![](https://img-blog.csdn.net/20170113102222871?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


  


### 命令行赋值 neato\_laser\_publisher


源码片：




```
1. std::string port;
2. int baud_rate;
3. std::string frame_id;
4. int firmware_number;
5. 
6. std_msgs::UInt16 rpms;
7. 
8. priv_nh.param("port", port, std::string("/dev/ttyUSB0"));
9. priv_nh.param("baud_rate", baud_rate, 115200);
10. priv_nh.param("frame_id", frame_id, std::string("neato_laser"));
11. priv_nh.param("firmware_version", firmware_number, 1);

```

  
 赋值命令： 
  



```
rosrun xv_11_laser_driver neato_laser_publisher **_port:=/dev/ttyUSB0 _firmware_version:=2**
```


```
rosrun mypkg mynode.py **_savepath:=/home/usrname/catkin_ws/src/mypkg/save_mm_dir**
```

  
 


### 命名空间与params


  
 


  
 


### 综合赋值问题实例 - Usage


  >> global\_name params : python节点代码、yaml赋值param、launchFile赋值param、命令行赋值param、rosparam工具赋值param  
 


  >> relative\_name params : python节点代码、yaml赋值param、launchFile赋值param、命令行赋值param、rosparam工具赋值param  
 


  >> private\_name params : python节点代码、yaml赋值param、launchFile赋值param、命令行赋值param、rosparam工具赋值param


------  
 


**Private**\_name params + PythonCode + LaunchFile\_Envalue:


![](https://img-blog.csdn.net/20170112133114218?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


  
 


  
 


  
 


  
 


  
 





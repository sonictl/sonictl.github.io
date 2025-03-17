---
layout: post
title:  "ROS 进阶学习笔记（14） - About driving your iRobot Roomba-Create"
date: 2016-04-26 11:00:02 +0800
tags: 
slug: p20160426110002
---

# ROS 进阶学习笔记（14） - About driving your iRobot Roomba-Create





## ROS 进阶学习笔记（14） - About driving your iRobot Roomba/Create


----------------  
 There are several  iRobot ROOMBA/CREATE drivers in ROS community, by May 2016, the newest one sholuld be the[create\_autonomy](https://github.com/autonomylab/create_autonomy) . Here we only discuss about the one used by the book " ROS by Example \_ hydro \_ vol1 ".  
 


----------------  
 这篇文章主要介绍使用下面的Create驱动上电不能充电问题。至于另2篇介绍iRobot Roomba作底盘驱动的博客参考：  
 http://blog.csdn.net/sonictl/article/details/49908949  
 http://blog.csdn.net/sonictl/article/details/50207949  
   
 


There are two important script file you may usually need to edit/check: turtlebot\_node.py & create\_driver.py


  
 


The turtlebot\_node.py is starting a ROS [node](https://so.csdn.net/so/search?q=node&spm=1001.2101.3001.7020) for controlling your turtlebot base, i.e, the iRobot Roomba® or iRobot Create®.


The create\_driver.py is a lower-level driver for driving your iRobot Roomba® or iRobot Create®.


The are working like this:


  
 


[Your other control message/topic sending nodes] ---->> [turtlebot\_node.py] ---->> [create\_driver.py]


Path:


    /opt/ros/hydro/lib/create\_node/turtlebot\_node.py  
 


    /opt/ros/hydro/lib/python2.7/dist-packages/create\_driver/create\_driver.py


  
 


Enjoy :)


    About turtlebot\_node.py, edit the set\_operation\_mode() function as below:




```
1. def set_operation_mode(self,req):
2. if not self.robot.sci:
3. rospy.logwarn("Create : robot not connected yet, sci not available")
4. return SetTurtlebotModeResponse(False)
5. 
6. self.operate_mode = req.mode
7. 
8. if req.mode == 1: #passive
9. rospy.logwarn("about to run passive mode, 1s...")
10. rospy.sleep(1.0)
11. self._robot_run_passive()
12. elif req.mode == 2: #safe
13. rospy.logwarn("about to run safe mode, 1s...")
14. rospy.sleep(1.0)
15. self._robot_run_safe()
16. elif req.mode == 3: #full
17. rospy.logwarn("about to run full mode, 1s...")
18. rospy.sleep(1.0)  #wait 1s so I can use Ctrl+C to halt it when I hope to charge my ROOMBA
19. self._robot_run_full()
20. else:
21. rospy.logwarn("Requested an invalid mode.")
22. return SetTurtlebotModeResponse(False)
23. return SetTurtlebotModeResponse(True)
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  

  


  
 





---
layout: post
title:  "ROS进阶学习手记 5 -- 使用Eclipse开发robot_cleaner"
date: 2015-07-21 10:18:48 +0800
tags: 
slug: p20150721101848
---

# ROS进阶学习手记 5 -- 使用Eclipse开发robot_cleaner





接上一节，


=======  开始用[Eclipse](https://so.csdn.net/so/search?q=Eclipse&spm=1001.2101.3001.7020)写一个发布给Turtlesim\_node驱动和角度Msg的 my\_node1 =====


参考Youtube上两个视频：


        [[CS460] ROS Tutorial 4.1 Turtlesim Cleaner Application - An Overview](https://www.youtube.com/watch?v=qtVDso-iBNA)


        [[CS460] ROS Tutorial 4.2 Moving in a Straight Line (Turtlesim Cleaner)](https://www.youtube.com/watch?v=PGZMlzBlMmw)  
 

 我们来在Eclipse里写这个node的代码。 
  

  
 


2015年7月21日09:59:26  
   
 建立一个名为robot\_cleaner的Package，生成robot\_cleaner\_node，向turtlesim\_node发送msg.  
 topic info:  
     topic\_name:    /turtle1/cmd\_vel  
     topic\_type:    geometry\_msgs/Twist  
 Message info:  
 



```
    $ rosmsg show geometry_msgs/Twist
```

    show the data struct:  
             geometry\_msgs/Vector3 linear  
               float64 x  
               float64 y  
               float64 z  
             geometry\_msgs/Vector3 angular  
               float64 x  
               float64 y  
               float64 z  
 



```
$ rostopic echo /turtle1/cmd_vel
```

得到：  
 



```
  linear: 
    x: 0.0
    y: 0.0
    z: 0.0
  angular: 
    x: 0.0
    y: 0.0
    z: 2.0
```

Package Dependency:  
               roscpp  
               rospy  
               std\_msgs  
               geometry\_msgs  
               message\_generation  
 ===============  
 


#### 1. Create the Package:



```
    $ cd ~/catkin_ws/src/
    $ catkin_create_pkg robot_cleaner     roscpp rospy std_msgs geometry_msgs message_generation

```

#### 2. Generate the Eclipse Files for dev. in Eclipse IDE



```
    $ cd ~/catkin_ws    //这实际上是Workspace的路径，catkin_ws = catkin workspace
    $ catkin_make --force-cmake -G"Eclipse CDT4 - Unix Makefiles"
    $ . ~/catkin_ws/devel/setup.bash
```

  
     to generate the .project file and then run:  
 



```
    $ awk -f $(rospack find mk)/eclipse.awk build/.project > build/.project_with_env && mv build/.project_with_env build/.project
```

   此时刷新Eclipse里的Project Explorer，可以看到“robot\_cleaner”这个项目。  
   【问题】： 为何要在Terminal里建立Package再生成Eclipse File，不能直接在Eclipse里创建Package么？怎么创建？  
 


#### 3. 在Eclipse里创建cpp源文件


   Project@Build -> [Source directory] -> robot\_cleaner ->src ->单击鼠标右键-> new ->file ->File name:robot\_cleaner.cpp  
    [如图片]:  
      ![](https://img-blog.csdn.net/20150721101826331?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
      
 


#### 4. 编辑cpp源文件


   



```
1. /* File: robot_cleaner.cpp
2. * source file
3. * auther : sonic
4. *
5. */
6. 
7. #include "ros/ros.h"
8. #include "geometry_msgs/Twist.h"
9. 
10. ros::Publisher velocity_publisher;  //Declare a ros Publisher: velocity_publisher
11. 
12. //Declare the method to move the robot straight
13. void move(double speed, double distance, bool isForward);
14. 
15. int main(int argc, char **argv){
16. //Initiate new ROS node named "talker"
17. ros::init(argc, argv, "robot_cleaner");
18. ros::NodeHandle n;
19. ROS_INFO("Your robot cleaner node starts!");  //打印，用于调试。
20. velocity_publisher = n.advertise<geometry_msgs::Twist>("/turtle1/cmd_vel",10);  //initiate the ros publisher
21. move(2.0, 5.0, 1);
22. 
23. }
24. 
25. //Definition of the method move()
26. void move(double speed, double distance, bool isForward){
27. geometry_msgs::Twist vel_msg;
28. //distance = speed * time
29. 
30. //set a random linear velocity in the x-axis
31. if (isForward)
32. vel_msg.linear.x = abs(speed);
33. else
34. vel_msg.linear.x = -abs(speed);
35. 
36. vel_msg.linear.y = 0;
37. vel_msg.linear.z = 0;
38. 
39. //set a random angular velocity in the y-axis
40. vel_msg.angular.x = 0;
41. vel_msg.angular.y = 0;
42. vel_msg.angular.z = 0;
43. 
44. //t0: the current time
45. double t0 = ros::Time::now().toSec();
46. double current_distance = 0;
47. ros::Rate loop_rate(10);
48. do{
49. velocity_publisher.publish(vel_msg);
50. double t1 = ros::Time::now().toSec();
51. current_distance = speed * (t1 - t0);
52. ros::spinOnce();
53. loop_rate.sleep();
54. }while (current_distance < distance);
55. vel_msg.linear.x = 0; //stop it
56. velocity_publisher.publish(vel_msg);
57. //loop
58. //publish the velocity
59. //estimate the distance = speed *(t1-t0)
60. //current_distance_moved_by_robot <= distance
61. 
62. 
63. }
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```



#### 5. 修改Catkin Make List文件


    文件： CMakelists.txt


    加入如下代码：



```
   add_executable(robot_cleaner_node src/robot_cleaner.cpp)    #specify which cpp should be compile and create the execute
   target_link_libraries(robot_cleaner_node ${catkin_LIBRARIES})
   add_dependencies(robot_cleaner_node robot_cleaner_gencpp)

```

    ![](https://img-blog.csdn.net/20150721133728321?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


#### 6. 编译、运行和调试你的executables


    Eclipse -> Project -> Build Project...  
 


#### 7. 运行和调试你的executables


    先来看看能不能动哈：


    $ rosrun turtlesim turtlesim\_node  
 


    $ rosrun robot\_cleaner robot\_cleaner\_node  
     我们看到robot\_cleaner\_node 打印出：“The robot cleaner [node](https://so.csdn.net/so/search?q=node&spm=1001.2101.3001.7020) starts!”， 并且乌龟向右移动了。  
     ![](https://img-blog.csdn.net/20150721113008447?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


  
 


   OK， 至此，我们就完成了一个可以发布geometry\_msg的node.  
 


练习1：让[turtle](https://so.csdn.net/so/search?q=turtle&spm=1001.2101.3001.7020)画个圆，如何？  
 练习2：让turtle走到pose = 特定位置的点，怎么弄？（涉及到从turtlesim\_node接收topic="/turtle1/pose"的信息，并实时规划linear.x 和 angular.z 的值）


欢迎通过评论提交各位的代码~ :)  
 


  
 




文章知识点与官方知识档案匹配，可进一步学习相关知识[Java技能树](https://edu.csdn.net/skill/java/?utm_source=csdn_ai_skill_tree_blog)[首页](https://edu.csdn.net/skill/java/?utm_source=csdn_ai_skill_tree_blog)[概览](https://edu.csdn.net/skill/java/?utm_source=csdn_ai_skill_tree_blog)146525 人正在系统学习中

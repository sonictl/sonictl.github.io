---
layout: post
title:  "ROS 进阶学习笔记(19)-- Add Extra Gyro Sensor for ROOMBA-Based Turtlebot"
date: 2016-06-17 17:33:35 +0800
tags: 
slug: p20160617173335
---

# ROS 进阶学习笔记(19)-- Add Extra Gyro Sensor for ROOMBA-Based Turtlebot





## ===Add Extra Gyro [Sensor](https://so.csdn.net/so/search?q=Sensor&spm=1001.2101.3001.7020) for ROOMBA-Based Turtlebot===


I write this in English for covering more friends globally. Please Indicate the Source[Sonictl\_blog\_csdn](http://blog.csdn.net/sonictl).
  

### == Background of this problem ==

 As I'm following the book "ros by example\_hydro\_vol1" written by R. Patrick Goeble, In order to follow the instances in that book, I implemented a turtlebot-like turtlebot based on the iRobot Create2/ROOMBA\_5xx/ROOMBA\_6xx/etc. One of the big differences should be that the original turtlebot is based on the "iRobot Create" which has a 25pin( 
[img](http://library.isr.ist.utl.pt/docs/roswiki/attachments/trike/bay_power.jpg)) connector and the driver for it "turtlebot\_driver"(ROS package) is written for that connector. ref 
 [link](http://www.irobot.com/filelibrary/pdfs/hrd/create/Create%20Open%20Interface_v2.pdf). But, the iRobot Create2/ROOMBA\_5xx/ROOMBA\_6xx/etc has no 25pin connector. 
  

  
 I guess that 25pin-connector receives analog voltage values and send back to the Controller(Ubuntu-PC) through the 7pin Mini-DIN Connector via the RS232-USART serial protocol on TTL voltage. (refer: 
[Calibrate Odometry and Gyro - ROS wiki](http://wiki.ros.org/turtlebot_calibration/Tutorials/Calibrate%20Odometry%20and%20Gyro) ) 
  

  

### == Background knowledge about this problem ==

 Before handling this, you should be a little familiar about ROS and ROS repositories. 
  
 You should have the items below: 
  

* Learned all the tutorials for ROS beginners and know what is stack/package/catkin/topic/node/msg/launch/parameter/../etc.
* Known basic concepts/ideas about OOP/C++/python


  

### == The structure of source code files about this problem ==

 As the background I have described, I'm following the rbx( 
***Ros by Example***) package and steps. But the rbx packages/launch\_files are developed based on 
[the turtlebot stacks](http://wiki.ros.org/turtlebot) (which contains 
turtlebot\_bringup<-turtlebot\_node<-turtlebot\_driver<-...) from ROS repositories. So, We will look into turtlebot stacks and find how does the gyro data processed/transferred by the packages of: 
turtlebot\_node, 
 turtlebot\_bringup and so on. Please Indicate the Source 
 [Sonictl\_blog\_csdn](http://blog.csdn.net/sonictl).
  

  

### == The structure of the source files that process the imu\_data ==

 I describe this structure by the following: 
  
   Start: User: launch rbx1\_bringup by : $ roslaunch rbx1\_bringup turtlebot\_minimal\_create.launch 
  
   Next: ROS : file( tb\_create\_mobile\_base.launch.xml ) is launching. 
  
 Let's look into this file: 
  
 There are two nodes about gyro\_sensor lauched: turtlebot\_node.py and robot\_pose\_ekf. And there are some parameters about gyro\_sensor: 
"has\_gyro"=true and 
"robot\_type"=roomba of turtlebot\_node, 
"imu\_used"=true and 
"output\_frame"=odom of robot\_pose\_ekf. Actually, most of the parameters of 
[robot\_pose\_ekf](http://wiki.ros.org/robot_pose_ekf) are used for gyros. 
  
   
  
 The imu/gyro's data is firstly got by turtlebot\_node.py, from the turtlebot\_driver.py and the 
[roomba\_sensor\_handler.py](https://github.com/code-iai/turtlebot_roomba/blob/master/turtlebot_node/src/turtlebot_node/roomba_sensor_handler.py) , via the 
sensor\_handler object. And the data is stored in the 
sensor\_state object which belongs to the 
TurtlebotSensorState() Class. We can see the source code of 
turtlebot\_node.py, and find out the relationships of them. Please Indicate the Source 
[Sonictl\_blog\_csdn](http://blog.csdn.net/sonictl).
  

  

(ps: /opt/ros/hydro/lib/python2.7/dist-packages/create\_node/ path, in your hydro-ROS installed ubuntu PC, stores the python source files mentioned above.)  
 (ps: /opt/ros/hydro/lib/create\_node/turtlebot\_node.py may meet your interests.)
  

  
 What's next? When the sensor\_state object got the sensor's data, it was assigned to the 
s object (See the "s = self.sensor\_state" in the turtlebot\_node.py 
[source code](https://github.com/Arkapravo/turtlebot/blob/master/turtlebot_node/nodes/turtlebot_node.py).). There is another 
\_gyro  object which takes "s" and called its 
publish() method. Let's see what is 
\_gyro object. Please Indicate the Source 
[Sonictl\_blog\_csdn](http://blog.csdn.net/sonictl).
  

  

### == The \_gyro object - who published the first topic of imu\_data ==

 In the turtlebot\_node.py source code, we find that "self.\_gyro = TurtlebotGyro()" declares an object "self.\_gyro" which belongs to 
TurtlebotGyro() Class (defined by file: [/opt/ros/hydro/lib/python2.7/dist-packages/create\_node/gyro.py]). It's the 
TurtlebotGyro() Class has a method: 
publish() we mentioned above, if you look into the 
gyro.py
[source file](https://github.com/code-iai/turtlebot_roomba/blob/master/turtlebot_node/src/turtlebot_node/gyro.py). 
  

  

*You feel complex? Heh. It's it. That's why it takes me a full day to figure this out. Let's go on...*
  

  

So, the publish() method of TurtlebotGyro() class takes the  sensor\_state.user\_analog\_input value and assigned it to  imu\_data.angular\_velocity.z after some simple reshaping calculations which are about the parametergyro\_measurement\_range and mathematics.

 Finally, the imu\_data is published by the publisher(Type:sensor\_msgs.msg.Imu Topic:imu/data & imu/raw) at line 81 and line 90 of the sourcefile(link:https://github.com/code-iai/turtlebot\_roomba/blob/master/turtlebot\_node/src/turtlebot\_node/gyro.py#L81) 
  

  

### == The flow of the imu\_data messages ==

 Once the 
imu/data topic is published out, the nodes that subscribe to it will be discussed here. 
  
 In file we mentioned above: 
tb\_create\_mobile\_base.launch.xml
  

    <remap from="imu/data" to="mobile\_base/sensors/imu\_data" />  
     <remap from="imu/raw" to="mobile\_base/sensors/imu\_data\_raw" />
  

So we check which node subscribes to "mobile\_base/sensors/imu\_data".


The first node subscribing to "mobile\_base/sensors/imu\_data" is [robot\_pose\_ekf], and [robot\_pose\_ekf] subscribes to /odom & /mobile\_base/sensors/imu\_data (tip: You can check this by command: $ rosnode -info /robot\_pose\_ekf), and publishes: /odom\_combined  
 


  
 In the rbx book, Chapter 7.6.4-Timed Out and Back using a Real Robot, the author asks us to launch the commands below: 
  

  $ roslaunch rbx1\_bringup turtlebot\_minimal\_create.launch
  
    (start the nodes to drive and publish/process sensor data) 
  

  

  $ roslaunch rbx1\_bringup odom\_ekf.launch
  
    (run the node "odom\_ekf" which belongs to rbx1\_bringup package, written by the author) 
  

  

  $ rosrun rviz rviz -d `rospack find rbx1\_nav`/nav\_ekf.rviz
  
    (run the .rviz file configured rviz to monitor our /odom\_ekf topic published by node "odom\_ekf") 
  

  
   $ rosrun rbx1\_nav timed\_out\_and\_back.py
  
    (make the turtlebot move so that we can see the effect better) 
  

  
 Finally we get the result of lovely orange arrows: 
![微笑](http://static.blog.csdn.net/xheditor/xheditor_emot/default/smile.gif)
  

![(figure: result.jpg)](https://img-blog.csdn.net/20160617172436980?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
  

  

  

### == The Steps of My Implementation of Adding Extra Gyro Sensor for ROOMBA ==

 I'm sorry the most wanted part is finally shown at the last order now. 
![吐舌头](http://static.blog.csdn.net/xheditor/xheditor_emot/default/tongue.gif)
  
 I'll show the steps concisely because I have described all the details above and have wasted you too much time. 
  

  

#### Step1:

 Get your gyro sensor hardware(chip? board? aduino?) connect to your Ubuntu-PC and make your sensor data can be published in ROS message on ROS topic('imu/angular\_vel' for instance). The ROS message should be of type of 
[geometry\_msgs.msg.Twist()](http://docs.ros.org/jade/api/sensor_msgs/html/msg/Imu.html). Please Indicate the Source 
[Sonictl\_blog\_csdn](http://blog.csdn.net/sonictl).
  

  

#### Step2:

 Modify and add subscriber in your turtlebot\_node.py source file: 
  
 for instance: 
  


```
        self.imu_sub=rospy.Subscriber("imu/angular_vel", Twist, self.callbackimu)
```

  
 This makes your 'turtlebot' node receive your gyro data published by Step1. In your callbackimu method, take the  data on topic 'imu/angular\_vel' and assign it to self.add\_gyro\_data.angular.z. The object self.add\_gyro\_data should be of type of geometry\_msgs.msg.Twist() obviously. The callbackimu method: 
  


```
      self.temp=Twist() #declare the Twist object for addGyroData when initialize

...
      def callbackimu(self, data):
          self.add_gyro_data.angular.z = data.angular.z
```

#### Step3:

 Modify the 
gyro.py source file and make the 
publish() method can have one more argument: 
  


```
        def publish(self, sensor_state, last_time, input_data)
```
        Absolutely, you should take input\_data to do some calculations and assign it to self.imu\_data.angular\_velocity.z for publishing it out. So, slightly modify the publish() method to meet your needs. Please Indicate the Source 
[Sonictl\_blog\_csdn](http://blog.csdn.net/sonictl).
  

  

#### Step4:

 In 
turtlebot\_node.py , Try to publish the 
 self.add\_gyro\_data object out by calling the 
self.\_gyro.publish() method. 
  


```
        if self._gyro:
            self._gyro.publish(s, last_time, self.add_gyro_data)
```

  

Check if you have achieved what you want, as the figure shown above. Please Indicate the Source[Sonictl\_blog\_csdn](http://blog.csdn.net/sonictl).


  
 


### Contact me

 You can write to me your comment or ideas via:     
sonictl    A-T    sina   D-O-T   com  (no spaces)
  

  

  

  




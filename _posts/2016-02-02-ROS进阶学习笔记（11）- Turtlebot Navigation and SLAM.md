---
layout: post
title: 'ROS进阶学习笔记（11）- Turtlebot Navigation and SLAM'
date: 2016-02-02 05:56:00 +0800
category: from_cnblogs
slug: p20160202055600
---


<p>(写在前面： 这里参考rbx书中第八章和ROS社区教程进行学习，先看<a target="_blank" target="_blank" href="http://wiki.ros.org/turtlebot_navigation/Tutorials">社区教程</a>)<br>
</p>
<p>&nbsp; ===&nbsp; Doing the Turtlebot Navigation&nbsp;&nbsp; === </p>
<p>ref ros wiki: http://wiki.ros.org/turtlebot_navigation/Tutorials</p>
<p><br>
</p>
<h3>1. Create the Data under remote control<br>
</h3>
<p>Referring the RBX book(8.4.2 Collecting and Recording Scan Data), <br>
then log into the TurtleBot's laptop and run:<br>
<span style="font-family:Courier New; color:#006600">&nbsp; $ roslaunch rbx1_bringup turtlebot_minimal_create.launch</span><br>
Replace the above command with the appropriate launch file for your own robot if you have one.<br>
Next, log into the TurtleBot using another terminal window and run the command:<br>
<span style="font-family:Courier New; color:#006600">&nbsp; $ roslaunch rbx1_bringup fake_laser.launch</span><br>
(If you have a real laser scanner, you would run its launch file here instead of the fake<br>
laser launch file.)<br>
Next, launch the gmapping_demo.launch launch file. You can launch this file on your desktop workstation or the robot's laptop:<br>
<span style="font-family:Courier New; color:#006600">&nbsp; $ roslaunch rbx1_nav gmapping_demo.launch</span><br>
&nbsp; or follow ros wiki: exbot@my_robot1:~$ roslaunch rbx1_nav gmapping_demo.launch<br>
<br>
Then bring up RViz with the included gmapping configuration file On_Desktop:<br>
<span style="font-family:Courier New; color:#990000">&nbsp; $ rosrun rviz rviz -d `rospack find rbx1_nav`/gmapping.rviz</span><br>
&nbsp; or follow ros wiki: exbot@my_desktop:~$ roslaunch turtlebot_rviz_launchers view_navigation.launch&nbsp; # I used this, cuz the above causes problem.<br>
<br>
Next, launch a teleop node On_Desktop for either the keyboard or joystick depending on your hardware:<br>
<span style="font-family:Courier New; color:#990000">&nbsp; $ roslaunch rbx1_nav keyboard_teleop.launch</span></p>
<p>The final step is to start recording the data to a bag file. You can create the file<br>
anywhere you like, but there is a folder called bag_files in the <em>rbx1_nav</em> package for<br>
this purpose if you want to use it:<br>
<span style="font-family:Courier New">&nbsp; $ roscd rbx1_nav/bag_files<br>
</span><br>
Now start the recording process:<br>
<span style="font-family:Courier New">&nbsp; $ rosbag record -O my_scan_data /scan /tf<br>
</span><br>
where <span style="font-family:Courier New">my_scan_data</span> can be any filename you like. The only data we need to record is<br>
the laser scan data and the <span style="font-family:Courier New">tf</span> transforms. (The<span style="font-family:Courier New">tf</span> transform tree includes the<br>
transformation from the <span style="font-family:Courier New">/odom</span> frame to the<span style="font-family:Courier New">/base_link</span> or<span style="font-family:Courier New">/base_footprint</span> frame<br>
which gives us the needed odometry data.)<br>
<br>
You are now ready to drive the robot around the area you'd like to map. Be sure to<br>
move the robot slowly, especially when rotating. Stay relatively close to walls and<br>
furniture so that there is always something within range of the scanner. Finally, plan to<br>
drive a closed loop and continue past the starting point for several meters to ensure a<br>
good overlap between the beginning and ending scan data.<br>
</p>
<p>Use the Steps above can let your turtlebot move under your control and create the map of the environment.</p>
<p><br>
</p>
<h3>2. Create the map simutanlously.</h3>
<p>There are at least two ways to create the map: Recording or rePlaying the bag data file. Let's see the first here and second next section.<br>
</p>
When you are finished driving the robot, type Ctrl-C in the rosbag terminal window<br>
to stop the recording process. Then save the current map as follows:<br>
<span style="font-family:Courier New">&nbsp; $ roscd rbx1_nav/maps<br>
&nbsp; $ rosrun map_server map_saver -f my_map</span><br>
where &quot;<span style="font-family:Courier New">my_map</span>&quot; can be any name you like. This will save the generated map into the<br>
current directory under the name you specified on the command line. If you look at the<br>
contents of the <span style="font-family:Courier New">rbx1_nav/maps</span> directory, you will see two new files:<span style="font-family:Courier New">my_map.pgm</span><br>
which is the map image and<span style="font-family:Courier New"> my_map.yaml </span>
that describes the dimensions of the map. It <br>
is this latter file that you will point to in subsequent launch files when you want to usethe map for navigation.
<p>To view the new map, you can use any image viewer program to bring up the <span style="font-family:Courier New">
.pgm</span> file <br>
created above. For example, to use the Ubuntu eog viewer (&quot;eye of Gnome&quot;) run the<br>
</p>
command:<br>
<span style="font-family:Courier New">&nbsp; $ roscd rbx1_nav/maps<br>
&nbsp; $ eog my_map.pgm</span><br>
<p>You can zoom the map using your scroll wheel or the &#43;/- buttons. <br>
</p>
<p>Here is a video demonstrating the gmapping process using Pi Robot and a Hokuyo laser scanner: http://youtu.be/7iIDdvCXIFM<br>
</p>
<h3>3. Create the map by bag data file.</h3>
<p></p>
<p>You can also create the map from the bag data you stored during the scanning phase<br>
above. This is a useful technique since you can try out different <span style="font-family:Courier New">
gmapping</span> parameters <br>
on the same scan data without having to drive the robot around again. <br>
当你想调试gmapping 参数的时候，不用每次都重新跑一遍机器人。</p>
<p>把所有的Terminal中运行的node,launch关掉；Next, turn on simulated time by setting the use_sim_time parameter to true:<br>
<span style="font-family:Courier New">&nbsp; $ rosparam set use_sim_time true</span><br>
</p>
<p>Then clear the move_base parameters and re-launch the gmapping_demo.launch file<br>
again:<br>
<span style="font-family:Courier New">&nbsp; $ rosparam delete /move_base</span><br>
<span style="font-family:Courier New">&nbsp; $ roslaunch rbx1_nav gmapping_demo.launch</span><br>
You can monitor the process in RViz using the gmapping configuration file:<br>
<span style="font-family:Courier New">&nbsp; $ rosrun rviz rviz -d `rospack find rbx1_nav`/gmapping.rviz</span><br>
Finally, play back your recorded data:<br>
<span style="font-family:Courier New">&nbsp; $ roscd rbx1_nav/bag_files</span><br>
<span style="font-family:Courier New">&nbsp; $ rosbag play my_scan_data.bag</span><br>
You will probably have to zoom and/or pan the display to keep the entire scan area in<br>
view.<br>
When the rosbag file has played all the way through, you save the generated map the<br>
same way we did with the live data:<br>
<span style="font-family:Courier New">&nbsp; $ roscd rbx1_nav/maps<br>
&nbsp; $ rosrun map_server map_saver -f my_map</span><br>
where &quot;<span style="font-family:Courier New">my_map</span>&quot; can be any name you like.<br>
<br>
This will save the generated map into the current directory under the name you specified on the command line. If you look at the<br>
contents of the rbx1_nav/maps directory, you will see two files: <span style="font-family:Courier New">
my_map.pgm</span> which is the map image and<span style="font-family:Courier New"> my_map.yaml</span>that describes the dimensions of the map. It is this latter file that you will point to in subsequent launch files when you want to use the map<br>
for navigation.<br>
</p>
<p>To view the map created, you can use any image viewer program to bring up the .pgm<br>
file created above. For example, to use the Ubuntu eog viewer (&quot;eye of Gnome&quot;) run<br>
the command:<br>
<span style="font-family:Courier New">&nbsp; $ roscd rbx1_nav/maps<br>
&nbsp; $ eog my_map.pgm</span><br>
You can zoom the map using your scroll wheel or the &#43;/- buttons.<br>
<span style="color:#990000">NOTE: Don't forget to reset the use_sim_time parameter after you are finished map<br>
building. Use the command:<br>
<span style="font-family:Courier New">&nbsp; $ rosparam set use_sim_time false</span></span><br>
</p>
<br>
<h3>4. Conclusion</h3>
<p>Now that your map is saved we will learn how to use it for localization in the next section.<br>
For additional details about gmapping, take a look at the<span style="font-family:Courier New"> gmapping_demo.launch</span> file<br>
in the <span style="font-family:Courier New">rbx1_nav/launch</span> directory.&nbsp; There you will see many parameters that can be<br>
tweaked if needed.&nbsp; This particular launch file is a copied from the <br>
<span style="font-family:Courier New">turtlebot_navigation</span> package and the folks at OSRG have already dialed in the<br>
settings that should work for you.&nbsp; To learn more about each parameter, you can check<br>
out the <a target="_blank" target="_blank" href="http://wiki.ros.org/gmapping">gmapping Wiki page</a>.<br>
</p>
<p><br>
</p>
<p>视频: 结构化环境中机器人导航 - Navigation with Structured Env.SLA&nbsp; <a target="_blank" target="_blank" href="https://www.youtube.com/watch?v=EwNl1cfNjt4">
https://youtu.be/EwNl1cfNjt4</a></p>
<p><br>
</p>
<p><a target="_blank" target="_blank" href="https://youtu.be/EwNl1cfNjt4"></a><br>
</p>
   

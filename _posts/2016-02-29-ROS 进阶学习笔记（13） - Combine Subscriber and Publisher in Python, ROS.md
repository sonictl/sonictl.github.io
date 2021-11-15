---
layout: post
title: 'ROS 进阶学习笔记（13） - Combine Subscriber and Publisher in Python, ROS'
date: 2016-02-29 06:10:00 +0800
category: from_cnblogs
slug: p20160229061000
---


<h1>Combine Subscriber and Publisher in Python, ROS<br>
</h1>
<p>This article will describe an example of Combining Subscriber and Publisher in Python in ROS programming.</p>
<p>This is very useful in ROS development.</p>
<p>We will also discuss briefly how to build and modify a catkin package which is written by Python.</p>
<ul>
<li>Create a catkin package with the command: catkin_create_pkg, under the path: ~/catkin_ws/src</li><li>Build it with the command: catkin_make, under the path: ~/catkin_ws/</li><li>Source the catkin setup file under devel folder:<br>
<pre>$ source ~/catkin_ws/devel/setup.bash</pre>
</li><li>modify the Python scripts file under the path: ~/catkin_ws/src/&lt;pkg_name&gt;/scripts/nodexxx.py</li><li>chmod &#43;x nodexxx.py</li><li>Run this package by Command:&nbsp; rosrun package_name nodexxx.py</li><li>Modify the CMakefile.txt for Python: <a target="_blank" target="_blank" href="http://wiki.ros.org/rospy_tutorials/Tutorials/Makefile">
Writing a ROS Python Makefile</a><br>
</li></ul>
<p>More about Create and Build catkin ROS package: <a target="_blank" target="_blank" href="http://blog.csdn.net/sonictl/article/details/46764855#t5">
This blog</a><br>
</p>
<p>The sourcecode for this Combining Subscriber and Publisher in Python is here:</p>
<p></p>
<pre code_snippet_id="1591986" snippet_file_name="blog_20160229_1_8422993" name="code" class="python">#!/usr/bin/env python
# License removed for brevity
&quot;&quot;&quot;
	learn to write Subscriber and Listener in one python script.
	Function Style
	Author: Sonic http://blog.csdn.net/sonictl
	Date: Feb 29, 2016	
&quot;&quot;&quot;
import rospy
from std_msgs.msg import String

def callback(data):
    rospy.loginfo(rospy.get_caller_id() + &quot;callback:I heard %s&quot;, data.data)
    #resp_str = &quot;resp_str: I heard: &quot; + data.data
    talker(data)

def listener():

    # In ROS, nodes are uniquely named. If two nodes with the same
    # node are launched, the previous one is kicked off. The
    # anonymous=True flag means that rospy will choose a unique
    # name for our &#39;listener&#39; node so that multiple listeners can
    # run simultaneously. http://blog.csdn.net/sonictl
    rospy.init_node(&#39;responsor&#39;, anonymous=True)

    rospy.Subscriber(&quot;uc0Response&quot;, String, callback)

    # spin() simply keeps python from exiting until this node is stopped
    rospy.spin()
	
def talker(data):
    pub = rospy.Publisher(&#39;uc0Command&#39;, String, queue_size=10)
    rospy.loginfo(&quot;talker:I heard %s&quot;, data.data)

    #while not rospy.is_shutdown():
    resp_str = &quot;resp_str: I heard: &quot; + data.data
    rospy.loginfo(resp_str)
    if data.data == &quot;cigit-pc\n&quot; :
        pub.publish(resp_str)
    else:
        rospy.loginfo(&quot;invalid seri data:&quot; + data.data)
		
if __name__ == &#39;__main__&#39;:
    #listener()
    try:
        listener()
        #talker()
    except rospy.ROSInterruptException:
        pass
</pre>
<p><br>
</p>
<p>20160614: I have to paste the next code for an other instance because this should be very good for learners:</p>
<p><pre name="code" class="python">#!/usr/bin/env python

&quot;&quot;&quot; odom_ekf.py - Version 0.1 2012-07-08
    Republish the /robot_pose_ekf/odom_combined topic which is of type 
    geometry_msgs/PoseWithCovarianceStamped as an equivalent message of
    type nav_msgs/Odometry so we can view it in RViz.
    Created for the Pi Robot Project: http://www.pirobot.org
    Copyright (c) 2012 Patrick Goebel.  All rights reserved.
    This program is free software; you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation; either version 2 of the License, or
    (at your option) any later version.5
    
    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details at:
    
    http://www.gnu.org/licenses/gpl.html
      
&quot;&quot;&quot;

import roslib; roslib.load_manifest(&#39;rbx1_nav&#39;)
import rospy
from geometry_msgs.msg import PoseWithCovarianceStamped
from nav_msgs.msg import Odometry

class OdomEKF():
    def __init__(self):
        # Give the node a name
        rospy.init_node(&#39;odom_ekf&#39;, anonymous=False)

        # Publisher of type nav_msgs/Odometry
        self.ekf_pub = rospy.Publisher(&#39;output&#39;, Odometry)
        
        # Wait for the /odom topic to become available
        rospy.wait_for_message(&#39;input&#39;, PoseWithCovarianceStamped)
        
        # Subscribe to the /robot_pose_ekf/odom_combined topic
        rospy.Subscriber(&#39;input&#39;, PoseWithCovarianceStamped, self.pub_ekf_odom)
        
        rospy.loginfo(&quot;Publishing combined odometry on /odom_ekf&quot;)
        
    def pub_ekf_odom(self, msg):
        odom = Odometry()
        odom.header = msg.header
        odom.child_frame_id = &#39;base_footprint&#39;
        odom.pose = msg.pose
        
        self.ekf_pub.publish(odom)
        
if __name__ == &#39;__main__&#39;:
    try:
        OdomEKF()
        rospy.spin()
    except:
        pass
        </pre>This program above is for Transfer a <span style="font-family:Courier New">
PoseWithCovarianceStamped</span> typed topic &quot;input&quot; into <span style="font-family:Courier New">
Odometry</span> typed topic which is named &quot;output&quot;. <br>
We can learn how to initialize a node, declare a publisher, subscriber and how to use
<span style="font-family:Courier New">wait_for_message()<span style="font-family:Arial"> function. The callback function pub_ekf_odom is called when input has data, and we can see how this function works to convert the data from&nbsp;<span style="font-family:Courier New">PoseWithCovarianceStamped</span>
 into&nbsp;<span style="font-family:Courier New">Odometry</span>.</span></span><br>
</p>
<br>
<p></p>
   

---
layout: post
title: 'ROS学习手记 - 6 使用ROS中的工具:rqt_console &amp; roslaunch &amp; rosed'
date: 2015-07-09 01:58:00 +0800
category: from_cnblogs
slug: p20150709015800
---


<p><br>
</p>
<p><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch">http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch</a><br>
</p>
<p></p>
<ol>
<li>
<h2><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch">Using rqt_console and roslaunch</a></h2>
<p><span class="anchor" id="line-1-8"></span>This tutorial introduces ROS using <a target="_blank" target="_blank" href="http://wiki.ros.org/rqt_console">
rqt_console</a> and <a target="_blank" target="_blank" href="http://wiki.ros.org/rqt_logger_level">
rqt_logger_level</a> for debugging and <a target="_blank" target="_blank" href="http://wiki.ros.org/roslaunch">
roslaunch</a> for starting many nodes at once. If you use <tt class="backtick">ROS&nbsp;fuerte</tt> or ealier distros where<a target="_blank" target="_blank" href="http://wiki.ros.org/rqt">rqt</a> isn't fully available, please see this page with<a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRxconsoleRoslaunch">this
 page</a> that uses old <tt class="backtick">rx</tt> based tools. </p>
</li></ol>
<p></p>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <br>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; rqt console工具的打开：</p>
<p></p>
<h3 id="Using_rqt_console_and_rqt_logger_level">Using rqt_console and rqt_logger_level</h3>
<span class="anchor" id="line-33"></span>
<p></p>
<p class="line867"><tt class="backtick">rqt_console</tt> attaches to ROS's logging framework to display output from nodes.<tt class="backtick">rqt_logger_level</tt> allows us to change the verbosity level (DEBUG, WARN, INFO, and ERROR) of nodes as they run.<span class="anchor" id="line-34"></span><span class="anchor" id="line-35"></span></p>
<p class="line862">Now let's look at the turtlesim output in <tt class="backtick">
rqt_console</tt> and switch logger levels in <tt class="backtick">rqt_logger_level</tt> as we use turtlesim. Before we start the turtlesim,<strong>in two new terminals</strong> start
<tt class="backtick">rqt_console</tt> and<tt class="backtick">rqt_logger_level</tt>:
<span class="anchor" id="line-36"></span><span class="anchor" id="line-37"></span></p>
<p class="line867"><span class="anchor" id="line-38"></span><span class="anchor" id="line-39"></span></p>
<pre><span class="anchor" id="line-1-3"></span>$ rosrun rqt_console rqt_console</pre>
<span class="anchor" id="line-40"></span>
<p class="line867"><span class="anchor" id="line-41"></span><span class="anchor" id="line-42"></span></p>
<pre><span class="anchor" id="line-1-4"></span>$ rosrun rqt_logger_level rqt_logger_level</pre>
这里面就可以看到正在运行的node的信息，message内容等 。
<p>logger level工具可以查看报警信息，注意logger level是有层级的，<tt class="backtick">Fatal</tt> has the highest priority and<tt class="backtick">Debug</tt> has the lowest. By setting the logger level, you will get all messages of that priority level or higher。</p>
<p class="line874">Logging levels are prioritized in the following order: <span class="anchor" id="line-77">
</span><span class="anchor" id="line-78"></span></p>
<p class="line867"><span class="anchor" id="line-79"></span><span class="anchor" id="line-80"></span><span class="anchor" id="line-81"></span><span class="anchor" id="line-82"></span><span class="anchor" id="line-83"></span><span class="anchor" id="line-84"></span></p>
<pre><span class="anchor" id="line-1-8"></span>Fatal
<span class="anchor" id="line-2-2"></span>Error
<span class="anchor" id="line-3-2"></span>Warn
<span class="anchor" id="line-4-2"></span>Info
<span class="anchor" id="line-5-2"></span>Debug</pre>
<p>使用roslaunch工具</p>
<p>&nbsp;&nbsp;&nbsp; roslaunch工具是按照launch文件开始运行nodes的工具，</p>
<p class="line874">Usage: <span class="anchor" id="line-93"></span><span class="anchor" id="line-94"></span></p>
<p class="line867"><span class="anchor" id="line-95"></span><span class="anchor" id="line-96"></span></p>
<pre><span class="anchor" id="line-1-9"></span>$ roslaunch [package] [filename.launch]</pre>
<pre>$ roslaunch beginner_tutorials turtlemimic.launch</pre>
&nbsp;&nbsp;&nbsp; 关于launch file :
<p>&nbsp;&nbsp;&nbsp; 位置：在package根目录下，有 src, include, launch文件夹，package.xml文件， 在launch文件夹下，建立finename.launch文件。</p>
<p>&nbsp;&nbsp;&nbsp; 文件的内容怎么写，见文后附。</p>
<p>&nbsp;&nbsp;&nbsp; <span style="color:#FF6666">这里有个问题是，roslaunch运行了这个文件以后，出来俩跟随的turtle。只能使用rostopic pub来驱动，为何不能用rosrun turtlesim turtle_teleop_key来驱动？</span></p>
<p></p>
<pre>$ rostopic pub /turtlesim1/turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, -1.8]'</pre>
<pre>$ rosrun turtlesim turtle_teleop_key</pre>
<br>
<p></p>
<p>========================================<br>
</p>
<h4 id="The_Launch_File">The Launch File</h4>
<span class="anchor" id="line-119"></span>
<p></p>
<p class="line874">Now let's create a launch file called turtlemimic.launch and paste the following:<span class="anchor" id="line-120"></span><span class="anchor" id="line-121"></span></p>
<p class="line867"><span class="anchor" id="line-122"></span><span class="anchor" id="line-123"></span><span class="anchor" id="line-124"></span><span class="anchor" id="line-125"></span><span class="anchor" id="line-126"></span><span class="anchor" id="line-127"></span><span class="anchor" id="line-128"></span><span class="anchor" id="line-129"></span><span class="anchor" id="line-130"></span><span class="anchor" id="line-131"></span><span class="anchor" id="line-132"></span><span class="anchor" id="line-133"></span><span class="anchor" id="line-134"></span><span class="anchor" id="line-135"></span><span class="anchor" id="line-136"></span><span class="anchor" id="line-137"></span><span class="anchor" id="line-138"></span><span class="anchor" id="line-139"></span><span class="anchor" id="line-1-13"></span></p>
<div class="highlight python">
<div class="codearea" dir="ltr" lang="en"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#" class="codenumbers">切换行号显示</a>
<pre dir="ltr" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777" lang="en"><span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_1">   1</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_1"></span><span class="anchor" id="line-1-14"></span>&lt;<span class="ID">launch</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_2">   2</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_2"></span><span class="anchor" id="line-2-5"></span></span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_3">   3</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_3"></span><span class="anchor" id="line-3-4"></span>  &lt;<span class="ID">group</span> <span class="ID">ns</span>=<span class="String">&quot;</span><span class="String">turtlesim1</span><span class="String">&quot;</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_4">   4</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_4"></span><span class="anchor" id="line-4-3"></span>    &lt;<span class="ID">node</span> <span class="ID">pkg</span>=<span class="String">&quot;</span><span class="String">turtlesim</span><span class="String">&quot;</span> <span class="ID">name</span>=<span class="String">&quot;</span><span class="String">sim</span><span class="String">&quot;</span> <span class="ResWord">type</span>=<span class="String">&quot;</span><span class="String">turtlesim_node</span><span class="String">&quot;</span>/&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_5">   5</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_5"></span><span class="anchor" id="line-5-3"></span>  &lt;/<span class="ID">group</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_6">   6</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_6"></span><span class="anchor" id="line-6-2"></span></span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_7">   7</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_7"></span><span class="anchor" id="line-7-2"></span>  &lt;<span class="ID">group</span> <span class="ID">ns</span>=<span class="String">&quot;</span><span class="String">turtlesim2</span><span class="String">&quot;</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_8">   8</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_8"></span><span class="anchor" id="line-8-2"></span>    &lt;<span class="ID">node</span> <span class="ID">pkg</span>=<span class="String">&quot;</span><span class="String">turtlesim</span><span class="String">&quot;</span> <span class="ID">name</span>=<span class="String">&quot;</span><span class="String">sim</span><span class="String">&quot;</span> <span class="ResWord">type</span>=<span class="String">&quot;</span><span class="String">turtlesim_node</span><span class="String">&quot;</span>/&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_9">   9</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_9"></span><span class="anchor" id="line-9-2"></span>  &lt;/<span class="ID">group</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_10">  10</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_10"></span><span class="anchor" id="line-10-2"></span></span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_11">  11</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_11"></span><span class="anchor" id="line-11-2"></span>  &lt;<span class="ID">node</span> <span class="ID">pkg</span>=<span class="String">&quot;</span><span class="String">turtlesim</span><span class="String">&quot;</span> <span class="ID">name</span>=<span class="String">&quot;</span><span class="String">mimic</span><span class="String">&quot;</span> <span class="ResWord">type</span>=<span class="String">&quot;</span><span class="String">mimic</span><span class="String">&quot;</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_12">  12</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_12"></span><span class="anchor" id="line-12-2"></span>    &lt;<span class="ID">remap</span> <span class="ID">from</span>=<span class="String">&quot;</span><span class="String">input</span><span class="String">&quot;</span> <span class="ID">to</span>=<span class="String">&quot;</span><span class="String">turtlesim1/turtle1</span><span class="String">&quot;</span>/&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_13">  13</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_13"></span><span class="anchor" id="line-13-2"></span>    &lt;<span class="ID">remap</span> <span class="ID">from</span>=<span class="String">&quot;</span><span class="String">output</span><span class="String">&quot;</span> <span class="ID">to</span>=<span class="String">&quot;</span><span class="String">turtlesim2/turtle1</span><span class="String">&quot;</span>/&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_14">  14</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_14"></span><span class="anchor" id="line-14-2"></span>  &lt;/<span class="ID">node</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_15">  15</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_15"></span><span class="anchor" id="line-15-2"></span></span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_16">  16</a> </span><span class="LineAnchor" id="CA-91a3946a9c4cf7301bb55ec0c3f8a77f6c8f9777_16"></span><span class="anchor" id="line-16-2"></span>&lt;/<span class="ID">launch</span>&gt;</span>
</pre>
</div>
</div>
<span class="anchor" id="line-140"></span>
<p class="line867"></p>
<h4 id="The_Launch_File_Explained">The Launch File Explained</h4>
<span class="anchor" id="line-141"></span>
<p class="line862">Now, let's break the launch xml down. <span class="anchor" id="line-1-15">
</span><span class="anchor" id="line-2-6"></span><span class="anchor" id="line-3-5"></span><span class="anchor" id="line-4-4"></span><span class="anchor" id="line-1-16"></span></p>
<div class="highlight python">
<div class="codearea" dir="ltr" lang="en"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#" class="codenumbers">切换行号显示</a>
<pre dir="ltr" id="CA-21ef414cf4c910bb1286ff2aedfe349a32a099b9" lang="en"><span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-21ef414cf4c910bb1286ff2aedfe349a32a099b9_1">   1</a> </span><span class="LineAnchor" id="CA-21ef414cf4c910bb1286ff2aedfe349a32a099b9_1"></span><span class="anchor" id="line-1-17"></span>&lt;<span class="ID">launch</span>&gt;</span>
</pre>
</div>
</div>
<span class="anchor" id="line-5-4"></span>
<p class="line874">Here we start the launch file with the launch tag, so that the file is identified as a launch file.<span class="anchor" id="line-142"></span><span class="anchor" id="line-143"></span></p>
<p class="line867"><span class="anchor" id="line-1-18"></span><span class="anchor" id="line-2-7"></span><span class="anchor" id="line-3-6"></span><span class="anchor" id="line-4-5"></span><span class="anchor" id="line-5-5"></span><span class="anchor" id="line-6-3"></span><span class="anchor" id="line-7-3"></span><span class="anchor" id="line-8-3"></span><span class="anchor" id="line-9-3"></span><span class="anchor" id="line-10-3"></span><span class="anchor" id="line-1-19"></span><span class="anchor" id="line-2-8"></span><span class="anchor" id="line-3-7"></span></p>
<div class="highlight python">
<div class="codearea" dir="ltr" lang="en"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#" class="codenumbers">切换行号显示</a>
<pre dir="ltr" id="CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64" lang="en"><span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_3">   3</a> </span><span class="LineAnchor" id="CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_3"></span><span class="anchor" id="line-1-20"></span>  &lt;<span class="ID">group</span> <span class="ID">ns</span>=<span class="String">&quot;</span><span class="String">turtlesim1</span><span class="String">&quot;</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_4">   4</a> </span><span class="LineAnchor" id="CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_4"></span><span class="anchor" id="line-2-9"></span>    &lt;<span class="ID">node</span> <span class="ID">pkg</span>=<span class="String">&quot;</span><span class="String">turtlesim</span><span class="String">&quot;</span> <span class="ID">name</span>=<span class="String">&quot;</span><span class="String">sim</span><span class="String">&quot;</span> <span class="ResWord">type</span>=<span class="String">&quot;</span><span class="String">turtlesim_node</span><span class="String">&quot;</span>/&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_5">   5</a> </span><span class="LineAnchor" id="CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_5"></span><span class="anchor" id="line-3-8"></span>  &lt;/<span class="ID">group</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_6">   6</a> </span><span class="LineAnchor" id="CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_6"></span><span class="anchor" id="line-4-6"></span></span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_7">   7</a> </span><span class="LineAnchor" id="CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_7"></span><span class="anchor" id="line-5-6"></span>  &lt;<span class="ID">group</span> <span class="ID">ns</span>=<span class="String">&quot;</span><span class="String">turtlesim2</span><span class="String">&quot;</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_8">   8</a> </span><span class="LineAnchor" id="CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_8"></span><span class="anchor" id="line-6-4"></span>    &lt;<span class="ID">node</span> <span class="ID">pkg</span>=<span class="String">&quot;</span><span class="String">turtlesim</span><span class="String">&quot;</span> <span class="ID">name</span>=<span class="String">&quot;</span><span class="String">sim</span><span class="String">&quot;</span> <span class="ResWord">type</span>=<span class="String">&quot;</span><span class="String">turtlesim_node</span><span class="String">&quot;</span>/&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_9">   9</a> </span><span class="LineAnchor" id="CA-0975bc12bed743bd6d6b7cf5af4a7bc4bf2fdd64_9"></span><span class="anchor" id="line-7-4"></span>  &lt;/<span class="ID">group</span>&gt;</span>
</pre>
</div>
</div>
<span class="anchor" id="line-11-3"></span>
<p class="line874">Here we start two groups with a namespace tag of turtlesim1 and turtlesim2 with a turtlesim node with a name of sim. This allows us to start two simulators without having name conflicts.<span class="anchor" id="line-144"></span><span class="anchor" id="line-145"></span></p>
<p class="line867"><span class="anchor" id="line-1-21"></span><span class="anchor" id="line-2-10"></span><span class="anchor" id="line-3-9"></span><span class="anchor" id="line-4-7"></span><span class="anchor" id="line-5-7"></span><span class="anchor" id="line-6-5"></span><span class="anchor" id="line-7-5"></span><span class="anchor" id="line-1-22"></span><span class="anchor" id="line-2-11"></span><span class="anchor" id="line-3-10"></span><span class="anchor" id="line-4-8"></span><span class="anchor" id="line-5-8"></span><span class="anchor" id="line-6-6"></span><span class="anchor" id="line-7-6"></span><span class="anchor" id="line-8-4"></span><span class="anchor" id="line-9-4"></span><span class="anchor" id="line-10-4"></span><span class="anchor" id="line-11-4"></span></p>
<div class="highlight python">
<div class="codearea" dir="ltr" lang="en"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#" class="codenumbers">切换行号显示</a>
<pre dir="ltr" id="CA-78308b822a594630211cae0b2b508b405d07b108" lang="en"><span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-78308b822a594630211cae0b2b508b405d07b108_11">  11</a> </span><span class="LineAnchor" id="CA-78308b822a594630211cae0b2b508b405d07b108_11"></span><span class="anchor" id="line-1-23"></span>  &lt;<span class="ID">node</span> <span class="ID">pkg</span>=<span class="String">&quot;</span><span class="String">turtlesim</span><span class="String">&quot;</span> <span class="ID">name</span>=<span class="String">&quot;</span><span class="String">mimic</span><span class="String">&quot;</span> <span class="ResWord">type</span>=<span class="String">&quot;</span><span class="String">mimic</span><span class="String">&quot;</span>&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-78308b822a594630211cae0b2b508b405d07b108_12">  12</a> </span><span class="LineAnchor" id="CA-78308b822a594630211cae0b2b508b405d07b108_12"></span><span class="anchor" id="line-2-12"></span>    &lt;<span class="ID">remap</span> <span class="ID">from</span>=<span class="String">&quot;</span><span class="String">input</span><span class="String">&quot;</span> <span class="ID">to</span>=<span class="String">&quot;</span><span class="String">turtlesim1/turtle1</span><span class="String">&quot;</span>/&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-78308b822a594630211cae0b2b508b405d07b108_13">  13</a> </span><span class="LineAnchor" id="CA-78308b822a594630211cae0b2b508b405d07b108_13"></span><span class="anchor" id="line-3-11"></span>    &lt;<span class="ID">remap</span> <span class="ID">from</span>=<span class="String">&quot;</span><span class="String">output</span><span class="String">&quot;</span> <span class="ID">to</span>=<span class="String">&quot;</span><span class="String">turtlesim2/turtle1</span><span class="String">&quot;</span>/&gt;</span>
<span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-78308b822a594630211cae0b2b508b405d07b108_14">  14</a> </span><span class="LineAnchor" id="CA-78308b822a594630211cae0b2b508b405d07b108_14"></span><span class="anchor" id="line-4-9"></span>  &lt;/<span class="ID">node</span>&gt;</span>
</pre>
</div>
</div>
<span class="anchor" id="line-8-5"></span>
<p class="line874">Here we start the mimic node with the topics input and output renamed to turtlesim1 and turtlesim2. This renaming will cause turtlesim2 to mimic turtlesim1.<span class="anchor" id="line-146"></span><span class="anchor" id="line-147"></span></p>
<p class="line867"><span class="anchor" id="line-1-24"></span><span class="anchor" id="line-2-13"></span><span class="anchor" id="line-3-12"></span><span class="anchor" id="line-4-10"></span><span class="anchor" id="line-1-25"></span><span class="anchor" id="line-2-14"></span><span class="anchor" id="line-3-13"></span><span class="anchor" id="line-4-11"></span><span class="anchor" id="line-5-9"></span><span class="anchor" id="line-6-7"></span><span class="anchor" id="line-7-7"></span><span class="anchor" id="line-8-6"></span><span class="anchor" id="line-9-5"></span><span class="anchor" id="line-10-5"></span><span class="anchor" id="line-11-5"></span><span class="anchor" id="line-12-3"></span><span class="anchor" id="line-13-3"></span><span class="anchor" id="line-14-3"></span><span class="anchor" id="line-15-3"></span><span class="anchor" id="line-16-3"></span></p>
<div class="highlight python">
<div class="codearea" dir="ltr" lang="en"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#" class="codenumbers">切换行号显示</a>
<pre dir="ltr" id="CA-e33435967e0cc6e3273fdf1ce957aba2daaf5023" lang="en"><span class="line"><span class="LineNumber"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRqtconsoleRoslaunch#CA-e33435967e0cc6e3273fdf1ce957aba2daaf5023_16">  16</a> </span><span class="LineAnchor" id="CA-e33435967e0cc6e3273fdf1ce957aba2daaf5023_16"></span><span class="anchor" id="line-1-26"></span>&lt;/<span class="ID">launch</span>&gt;</span>
</pre>
</div>
</div>
<span class="anchor" id="line-5-10"></span>This closes the xml tag for the launch file.<br>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<h2 id="Using_rosed">2. Using rosed</h2>
<span class="anchor" id="line-30"></span>
<p class="line867"><tt class="backtick">rosed</tt> is part of the <a target="_blank" href="http://wiki.ros.org/rosbash">
rosbash</a> suite. It allows you to directly edit a file within a package by using the package name rather than having to type the entire path to the package.
<span class="anchor" id="line-31"></span><span class="anchor" id="line-32"></span></p>
<p class="line874">Usage: <span class="anchor" id="line-33"></span><span class="anchor" id="line-34"></span></p>
<p class="line867"><span class="anchor" id="line-35"></span><span class="anchor" id="line-36"></span></p>
<pre><span class="anchor" id="line-1-2"></span>$ rosed [package_name] [filename]</pre>
<span class="anchor" id="line-37"></span>
<p class="line874">Example: <span class="anchor" id="line-38"></span><span class="anchor" id="line-39"></span></p>
<p class="line867"><span class="anchor" id="line-40"></span><span class="anchor" id="line-41"></span></p>
<pre><span class="anchor" id="line-1-3"></span>$ rosed roscpp Logger.msg</pre>
<span class="anchor" id="line-42"></span>
<p class="line874">This example demonstrates how you would edit the Logger.msg file within the roscpp package.
<span class="anchor" id="line-43"></span><span class="anchor" id="line-44"></span></p>
<p class="line862">If this example doesn't work is probably because you don't have the
<tt class="backtick">vim</tt> editor installed. Please refer to <a target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UsingRosEd#Editor">
Editor</a> section. If you don't know how to get out of vim, <a target="_blank" class="http" href="http://kb.iu.edu/data/afcz.html">
click here</a>. <span class="anchor" id="line-45"></span><span class="anchor" id="line-46"></span></p>
<p class="line874">If the filename is not uniquely defined within the package, a menu will prompt you to choose which of the possible files you want to edit.
<span class="anchor" id="line-47"></span><span class="anchor" id="line-48"></span><span class="anchor" id="line-49"></span></p>
<p class="line867"></p>
<h3 id="Using_rosed_with_tab_completion">Using rosed with tab completion</h3>
<span class="anchor" id="line-50"></span>
<p class="line874">This way you can easily see and optionally edit all files from a package without knowing its exact name.
<span class="anchor" id="line-51"></span><span class="anchor" id="line-52"></span></p>
<p class="line874">Usage: <span class="anchor" id="line-53"></span><span class="anchor" id="line-54"></span></p>
<p class="line867"><span class="anchor" id="line-55"></span><span class="anchor" id="line-56"></span></p>
<pre><span class="anchor" id="line-1-4"></span>$ rosed [package_name] &lt;tab&gt;&lt;tab&gt;</pre>
<span class="anchor" id="line-57"></span>
<p class="line874">Example: <span class="anchor" id="line-58"></span><span class="anchor" id="line-59"></span><span class="anchor" id="line-60"></span></p>
<pre><span class="anchor" id="line-1-5"></span>$ rosed roscpp &lt;tab&gt;&lt;tab&gt;</pre>
<span class="anchor" id="line-61"></span>
<ul>
<li style="list-style-type:none"><span class="anchor" id="line-62"></span><span class="anchor" id="line-63"></span><span class="anchor" id="line-64"></span><span class="anchor" id="line-65"></span><span class="anchor" id="line-66"></span><span class="anchor" id="line-67"></span><span class="anchor" id="line-68"></span><span class="anchor" id="line-69"></span>
<pre><span class="anchor" id="line-1-6"></span>Empty.srv                   package.xml
<span class="anchor" id="line-2-2"></span>GetLoggers.srv              roscpp-msg-extras.cmake
<span class="anchor" id="line-3-2"></span>Logger.msg                  roscpp-msg-paths.cmake
<span class="anchor" id="line-4-2"></span>SetLoggerLevel.srv          roscpp.cmake
<span class="anchor" id="line-5-2"></span>genmsg_cpp.py               roscppConfig-version.cmake
<span class="anchor" id="line-6-2"></span>gensrv_cpp.py               roscppConfig.cmake
<span class="anchor" id="line-7-2"></span>msg_gen.py                  </pre>
<span class="anchor" id="line-70"></span><span class="anchor" id="line-71"></span></li></ul>
<p class="line867"></p>
<h3 id="Editor">Editor</h3>
<span class="anchor" id="line-72"></span><span class="anchor" id="line-73"></span>
<p class="line862">The default editor for rosed is <tt class="backtick">vim</tt>. The more beginner-friendly editor
<tt class="backtick">nano</tt> is included with the default Ubuntu install. You can use it by editing your ~/.bashrc file to include:
<span class="anchor" id="line-74"></span><span class="anchor" id="line-75"></span><span class="anchor" id="line-76"></span></p>
<pre><span class="anchor" id="line-1-7"></span>export EDITOR='nano -w'</pre>
<span class="anchor" id="line-77"></span><span class="anchor" id="line-78"></span>
<p class="line862">To set the default editor to <tt class="backtick">emacs</tt> you can edit your ~/.bashrc file to include:
<span class="anchor" id="line-79"></span><span class="anchor" id="line-80"></span><span class="anchor" id="line-81"></span></p>
<pre><span class="anchor" id="line-1-8"></span>export EDITOR='emacs -nw'</pre>
<span class="anchor" id="line-82"></span><span class="anchor" id="line-83"></span>
<p class="line867"><em><strong>NOTE:</strong> changes in .bashrc will only take effect for new terminals. Terminals that are already open will not see the new environmental variable.</em>
<span class="anchor" id="line-84"></span><span class="anchor" id="line-85"></span></p>
<p class="line862">Open a new terminal and see if <tt>EDITOR</tt> is defined: <span class="anchor" id="line-86">
</span><span class="anchor" id="line-87"></span></p>
<p class="line867"><span class="anchor" id="line-88"></span><span class="anchor" id="line-89"></span></p>
<pre><span class="anchor" id="line-1-9"></span>$ echo $EDITOR</pre>
<span class="anchor" id="line-90"></span>
<ul>
<li style="list-style-type:none"><span class="anchor" id="line-91"></span><span class="anchor" id="line-92"></span>
<pre><span class="anchor" id="line-1-10"></span>nano -w</pre>
<span class="anchor" id="line-93"></span>or <span class="anchor" id="line-94"></span><span class="anchor" id="line-95"></span><span class="anchor" id="line-96"></span>
<pre><span class="anchor" id="line-1-11"></span>emacs -nw</pre>
<span class="anchor" id="line-97"></span><span class="anchor" id="line-98"></span></li></ul>
Now that you have successfully configured and used rosed, let's <a target="_blank" href="http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv">
create a Msg and Srv</a>.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<p><br>
</p>
   

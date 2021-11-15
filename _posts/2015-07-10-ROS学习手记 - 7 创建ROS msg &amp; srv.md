---
layout: post
title: 'ROS学习手记 - 7 创建ROS msg &amp; srv'
date: 2015-07-10 06:02:00 +0800
category: from_cnblogs
slug: p20150710060200
---


<p><br>
</p>
<p>至此，我们初步学习了ROS的基本工具，接下来一步步理解ROS的各个工作部件的创建和工作原理。</p>
<p>本文的详细文档：<a target="_blank" href="http://wenku.baidu.com/view/623f41b3376baf1ffd4fad7a">http://wenku.baidu.com/view/623f41b3376baf1ffd4fad7a</a><br>
</p>
<p></p>
<ol>
<li>
<h2><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv">Creating a ROS msg and srv</a></h2>
<p><span class="anchor" id="line-1-10"></span>This tutorial covers how to create and build msg and srv files as well as the<a target="_blank" target="_blank" href="http://wiki.ros.org/rosmsg">rosmsg</a>, rossrv and roscp commandline tools.</p>
</li></ol>
1. msg &amp; srv 文件介绍<br>
&nbsp;&nbsp;&nbsp; msg 文件 就是用来让不同语言能产生message的一个指导性文本文件。<br>
&nbsp;&nbsp;&nbsp; srv file describes a service. It is composed of two parts: a request and a response.<br>
<br>
2. msg &amp; srv 文件存储位置：<br>
&nbsp;&nbsp;&nbsp; &lt;workspace&gt;/packagename/msg/ <br>
&nbsp;&nbsp;&nbsp; &amp;<br>
&nbsp;&nbsp;&nbsp; &lt;workspace&gt;/packagename/srv/
<table border="1" cellpadding="0" cellspacing="0" height="1139" width="698">
<tbody>
<tr>
<td>&nbsp;</td>
<td>&nbsp;</td>
</tr>
<tr>
<td>==== msg file 说明 ====<br>
3. msgfile 的&#26684;式和生成<br>
&nbsp;&nbsp;&nbsp; 生成：<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $ cd ~/catkin_ws/src/beginner_tutorials<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $ mkdir msg<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $ echo &quot;int64 num&quot; &gt; msg/Num.msg<br>
<br>
&nbsp;&nbsp;&nbsp; &#26684;式：<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; string first_name<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; string last_name<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; uint8 age<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; uint32 score<br>
<br>
4. msgfile 添加到package.xml的dependency中<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;build_depend&gt;message_generation&lt;/build_depend&gt;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;run_depend&gt;message_runtime&lt;/run_depend&gt;<br>
<br>
5. 对CMakeLists.txt文件的修改：<br>
<br>
&nbsp; 5.1 添加message_generation dependency到CMakeLists.txt中：<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; message_generation before the closing parenthesis<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; find_package(catkin REQUIRED COMPONENTS<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; roscpp<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; rospy<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; std_msgs<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; message_generation&nbsp; // add ’message_generation’<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; )<br>
<br>
&nbsp; 5.2 export the message runtime dependency. <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; catkin_package(<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ...<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; CATKIN_DEPENDS message_runtime ...<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ...)<br>
<br>
&nbsp; 5.3 添加msg文件<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; add_message_files(<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; FILES<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Num.msg<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; )<br>
&nbsp;&nbsp;&nbsp; 添加了.msg文件在CMake里，我们保证在你添加其他.msg文件时，CMake知道何时它需要reconfigure重新配置项目。<br>
<br>
&nbsp; 5.4 确保generate_messages 函数被调用<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; generate_messages(<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; DEPENDENCIES<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; std_msgs<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; )<br>
<br>
6. rosmsg 工具的使用<br>
&nbsp;&nbsp;&nbsp; make sure that ROS can see it using the rosmsg show command.<br>
&nbsp;&nbsp;&nbsp; Usage:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $ rosmsg show [message type]<br>
&nbsp;&nbsp;&nbsp; Example: <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $ rosmsg show beginner_tutorials/Num<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $ rosmsg show Num<br>
===================</td>
<td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; === srv file 说明 ===<br>
&nbsp;1. srv的路径和生成<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $ roscd beginner_tutorials<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $ mkdir srv<br>
&nbsp;&nbsp;&nbsp; Instead of creating a new srv definition by hand, we will copy an existing one from another package.<br>
For that, roscp is a useful commandline tool for copying files from one package to another.<br>
&nbsp;&nbsp;&nbsp; 使用roscp工具从一个package拷贝文件到另一个。<br>
&nbsp;&nbsp;&nbsp; Usage: <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $ roscp [package_name] [file_to_copy_path] [copy_path]<br>
&nbsp;&nbsp;&nbsp; Now we can copy a service from the rospy_tutorials package: <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $ roscp rospy_tutorials AddTwoInts.srv srv/AddTwoInts.srv<br>
<br>
&nbsp;2. 保证package.xml里有dependency配置（可能独立修改了srv，确保一下）<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;build_depend&gt;message_generation&lt;/build_depend&gt;<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;run_depend&gt;message_runtime&lt;/run_depend&gt;<br>
<br>
&nbsp;3.1 add the message_generation dependency to generate messages in CMakeLists.txt:<br>
# Do not just add this line to your CMakeLists.txt, modify the existing line<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; find_package(catkin REQUIRED COMPONENTS<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; roscpp<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; rospy<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; std_msgs<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; message_generation<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; )<br>
<br>
&nbsp; 3.2 添加 srvice 文件<br>
&nbsp;&nbsp;&nbsp; add_service_files(<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; FILES<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; AddTwoInts.srv<br>
&nbsp;&nbsp;&nbsp; )<br>
<br>
&nbsp; 3.3 确保generate_messages 函数被调用<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; generate_messages(<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; DEPENDENCIES<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; std_msgs<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; )<br>
<br>
4. 使用rossrv工具<br>
&nbsp;&nbsp;&nbsp; 添加以后，确保ROS can see it using the rossrv show command. <br>
&nbsp;&nbsp;&nbsp; Usage: <br>
&nbsp;&nbsp;&nbsp; $ rossrv show &lt;service type&gt;<br>
&nbsp;&nbsp;&nbsp; Example: <br>
&nbsp;&nbsp;&nbsp; $ rossrv show beginner_tutorials/AddTwoInts<br>
&nbsp;&nbsp;&nbsp; You will see: <br>
&nbsp;&nbsp;&nbsp; int64 a<br>
&nbsp;&nbsp;&nbsp; int64 b<br>
&nbsp;&nbsp;&nbsp; ---<br>
&nbsp;&nbsp;&nbsp; int64 sum<br>
&nbsp;&nbsp;&nbsp; Similar to rosmsg, you can find service files like this without specifying package name:<br>
&nbsp;&nbsp;&nbsp; $ rossrv show AddTwoInts<br>
&nbsp;&nbsp;&nbsp; [beginner_tutorials/AddTwoInts]:<br>
&nbsp;&nbsp;&nbsp; int64 a<br>
&nbsp;&nbsp;&nbsp; int64 b<br>
&nbsp;&nbsp;&nbsp; ---<br>
&nbsp;&nbsp;&nbsp; int64 sum<br>
<br>
&nbsp;&nbsp;&nbsp; [rospy_tutorials/AddTwoInts]:<br>
&nbsp;&nbsp;&nbsp; int64 a<br>
&nbsp;&nbsp;&nbsp; int64 b<br>
&nbsp;&nbsp;&nbsp; ---<br>
&nbsp;&nbsp;&nbsp; int64 sum</td>
</tr>
</tbody>
</table>
<h3>无论是修改了msg还是srv，都需要做的步骤：</h3>
&nbsp; CMakeLists.txt文件的修改<br>
&nbsp; 1. <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; find_package(catkin REQUIRED COMPONENTS<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; roscpp<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; rospy<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; std_msgs<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; message_generation&nbsp; // add ’message_generation’<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; )<br>
<br>
&nbsp; 2. add any packages you depend on which contain .msg files that your messages use (in this case std_msgs), such that it looks like this:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; generate_messages(<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; DEPENDENCIES<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; std_msgs<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; )<br>
<br>
<h3>终于到了catkin_make出手的时候：</h3>
<p align="left">Now that we have made some new messages we need to make our package again:</p>
<p># In your catkin workspace (by default:~/catkin_ws$)</p>
<pre>$ cd ../..</pre>
<pre>$ catkin_make</pre>
<pre>$ cd -</pre>
<p align="left">至此，我们就编译完成了一套ROS 的 msg &amp; srv<br>
Any .msg file in the msg directory will generate code for use in all supportedlanguages.<br>
The C&#43;&#43; message header file will be generated in ~/catkin_ws/devel/include/beginner_tutorials/.<br>
The Python script will be created in ~/catkin_ws/devel/lib/python2.7/dist-packages/beginner_tutorials/msg.<br>
The lisp file appears in ~/catkin_ws/devel/share/common-lisp/ros/beginner_tutorials/msg/.</p>
<p align="left">The full specification for the message format is available at the<a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Message_Description_Language"><span style="color:blue">Message Description Language</span></a> page.</p>
<p align="left"><strong>Getting Help</strong></p>
<p align="left">We've seen quite a few ROS tools already. It can be difficult to keeptrack of what arguments each command requires. Luckily, most ROS tools providetheir own help.</p>
<p align="left">Try: </p>
<p>$ rosmsg -h</p>
<p align="left">You should see a list of different rosmsg subcommands. </p>
<p>Commands:</p>
<ul>
<li>
<pre>&nbsp; rosmsgshow Show message description</pre>
</li><li>
<pre>&nbsp; rosmsgusers&nbsp; Find files that use message</pre>
</li><li>
<pre>&nbsp; rosmsgmd5&nbsp; Display message md5sum</pre>
</li><li>
<pre>&nbsp; rosmsgpackage&nbsp; List messages in a package</pre>
</li><li>
<pre>&nbsp; rosmsgpackages List packages that contain messages</pre>
</li></ul>
<p align="left">You can also get help for subcommands </p>
<p align="left">$ rosmsg show -h</p>
<ul type="disc">
<li>This shows the arguments that are needed for rosmsg show: </li></ul>
<p align="left">·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Usage: rosmsg show[options] &lt;message type&gt;</p>
<p align="left">·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</p>
<p align="left">·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Options:</p>
<pre align="left">·&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-h, --help&nbsp;show this help message and exit</pre>
<pre align="left">&nbsp; -r, --raw&nbsp;&nbsp; show raw message text, including comments</pre>
<h3 align="left"><strong>Review</strong></h3>
<p align="left">Lets just list some of the commands we've used so far: </p>
<ul type="disc">
<li>rospack = ros&#43;pack(age) : provides information related to ROS packages </li><li>roscd = ros&#43;cd : <strong>c</strong>hanges <strong>d</strong>irectory to a ROS package or stack</li><li>rosls = ros&#43;ls : <strong>l</strong>ist<strong>s</strong> files in a ROS package</li><li>roscp = ros&#43;cp : <strong>c</strong>o<strong>p</strong>ies files from/to a ROS package</li><li>rosmsg = ros&#43;msg : provides information related to ROS message definitions </li><li>rossrv = ros&#43;srv : provides information related to ROS service definitions </li><li>catkin_make : makes (compiles) a ROS package
<ul type="circle">
<li>rosmake = ros&#43;make : makes (compiles) a ROS package (if you're not using a catkin workspace)</li></ul>
</li></ul>
<p align="left"><strong>Next Tutorial</strong></p>
<p align="left">Now that you've made a new ROS msg and srv, let's look at writing a simplepublisher and subscriber<a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28python%29"><span style="color:blue">(python)</span></a><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29"><span style="color:blue">(c&#43;&#43;)</span></a>.</p>
<br>
<p></p>
<p><br>
</p>
<p><br>
</p>
   

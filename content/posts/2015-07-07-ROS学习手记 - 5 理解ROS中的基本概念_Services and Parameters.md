---
layout: post
title: 'ROS学习手记 - 5 理解ROS中的基本概念_Services and Parameters'
date: 2015-07-07 02:30:00 +0800
category: from_cnblogs
slug: p20150707023000
---


<p>上一节完成了对nodes, Topic的理解，再深入一步： Services and Parameters</p>
<p><span style="color:#990000">我不理解为何 </span><span style="color:#990000">ROS wiki</span><span style="color:#990000"> 要把service与parameter放在一起介绍, 很想分开说</span>，但限于 csdn blog 没有文章顺序调整功能。只能罢了~~<br>
</p>
<p>-----------------以下是我作的关于ROS Service的总结-------------------</p>
<h2>关于ROS Service的总结：<br>
</h2>
<ul>
<li>
<p><span style="color:#990000"><strong>什么是ROS Service:</strong></span> 在wiki/tutorials/1.7 中，有“<a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UnderstandingServicesParams">Understanding ROS Services and Parameters</a>”一节， 我不理解为何要把service与parameter放在一起介绍。<br>
[概念concepts] ROS Service: Another way that Nodes communicate with nodes. A_node send a request to B_node, and B_node give a response.<br>
[命令Commands] sorservice list/call/type/find/uri&nbsp;&nbsp;&nbsp; &amp;&nbsp;&nbsp; rossrv - this command is mainly for the .srv files operations<br>
</p>
</li><li><strong><span style="color:#990000">什么定义了ROS Service:</span></strong> *.srv file - srv文件： 在<a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv">wiki/tutorials/1.10</a>&nbsp; 中，有“Creating Msg &amp; Srv” 一节，我还是没理解为何要把srv文件和msg文件放在一起介绍。<br>
The function of srv file: Discribe/define the data type for a service. The data type of Req. &amp; Resp. divided by '---'<br>
the srv file typically stored under ../workspace/src/package_name/srv/ path.<br>
[Create the srv file]: step1. echo &quot;...&quot; ./srv/filename.srv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; step2. uncomment &quot;gen_srv()&quot; in CMakeList.txt file.<br>
[Use the srv file]: $rossrv show filename<del>.srv</del> , rossrv -- info of service definitions</li><li><span style="color:#990000"><strong>什么程序实现了ROS Servie:</strong></span> 在 wiki/tutorials/1.14 中，有“<a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">Writing a simpleService and Client(C&#43;&#43;)</a>” 一节,
 其中详细介绍了Service_node &amp; Client_node的程序实现。<br>
</li></ul>
<p>-----------------以上是我作的关于ROS Service的总结-------------------</p>
<p></p>
<h2>关于ROS Parameters的总结：<br>
</h2>
<p></p>
<ul>
<li>
<p><span style="color:#990000"><strong>什么是ROS Parameters: </strong><span style="color:#000000"><span style="background-color:rgb(255,255,255)">ROS Parameters 是ROS程序运行时所需要的参数，比如你的机器人的轮子的半径，你的gyro传感器是否具备等。</span></span><strong><br>
</strong></span></p>
</li><li><span style="color:#990000"><strong>如何修改和操作ROS Parameters:</strong> <span style="color:#000000">
这里我要引用一些高级的东东，<a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UnderstandingServicesParams#Using_rosparam">简单的</a>就不说了。<a target="_blank" target="_blank" href="http://answers.ros.org/question/11687/gyro-and-odometry-calibration-not-effective/?answer=17320#post-id-17320">link</a><span style="color:#000000">
 是关于改了parameter没反应时的方法。还有要注意：<br>
<span style="font-family:Courier New">rosparam set use_sim_time true<br>
rosparam delete /move_base</span><br>
以上俩关于rosparam 命令的使用方法mentioned in this blog: <a target="_blank" target="_blank" href="http://blog.csdn.net/sonictl/article/details/50388569">
link</a></span></span><strong><br>
</strong></span></li><li>
<p><span style="color:#990000"><strong>如何在命令行中指派parameter的&#20540;：</strong><span style="color:#000000">这个叫“Dynamic Reconfigure” ，具体方法：1. Python:
<a target="_blank" href="http://wiki.ros.org/ROSNodeTutorialPython#Using_Parameter_Server_and_Dynamic_Reconfigure">
Link</a> &nbsp; 2. C&#43;&#43; <a target="_blank" href="http://wiki.ros.org/ROSNodeTutorialC%2B%2B#Using_Parameter_Server_and_Dynamic_Reconfigure">
Link</a> <br>
</span></span></p>
</li><li><span style="color:#990000"><span style="color:#000000"><br>
</span></span></li></ul>
<p>-----------------以下是我最初学习它的时候的手记-----------------<br>
</p>
<ol>
<li><span style="font-size:24px"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/UnderstandingServicesParams">Understanding ROS Services and Parameters</a></span>
<p><span class="anchor" id="line-1-7"></span>This tutorial introduces ROS services, and parameters as well as using the<a target="_blank" target="_blank" href="http://wiki.ros.org/rosservice">rosservice</a> and<a target="_blank" target="_blank" href="http://wiki.ros.org/rosparam">rosparam</a>
 commandline tools.</p>
</li></ol>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Description: This tutorial introduces ROS services, and parameters as well as using the rosservice and rosparam commandline tools.<br>
<br>
<h3>ROS Services</h3>
&nbsp;&nbsp;&nbsp; Services are another way that nodes can communicate with each other. Services allow nodes to send a request and receive a response.<br>
<br>
<h3>Using rosservice</h3>
<br>
*rosservice* can easily attach to ROS's client/service framework with services. rosservice has many commands that can be used on topics, as shown below:<br>
<br>
Usage:<br>
&nbsp;&nbsp;&nbsp; rosservice list&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; print information about active services<br>
&nbsp;&nbsp;&nbsp; rosservice call&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; call the service with the provided args<br>
&nbsp;&nbsp;&nbsp; rosservice type&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; print service type<br>
&nbsp;&nbsp;&nbsp; rosservice find&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; find services by service type<br>
&nbsp;&nbsp;&nbsp; rosservice uri&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; print service ROSRPC uri<br>
<br>
&nbsp;&nbsp;&nbsp; list<br>
&nbsp;&nbsp;&nbsp; type<br>
&nbsp;&nbsp;&nbsp; call<br>
&nbsp;&nbsp; &nbsp;<br>
<br>
<h3>Using rosparam</h3>
<br>
*rosparam* allows you to store and manipulate data on the ROS Parameter Server. The Parameter Server can store integers, floats, boolean, dictionaries, and lists. rosparam uses the YAML markup language for syntax. In simple cases, YAML looks very natural: 1
 is an integer, 1.0 is a float, one is a string, true is a boolean, [1, 2, 3] is a list of integers, and {a: b, c: d} is a dictionary. rosparam has many commands that can be used on parameters, as shown below:<br>
<br>
Usage:<br>
<br>
&nbsp;&nbsp;&nbsp; rosparam set&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; set parameter<br>
&nbsp;&nbsp;&nbsp; rosparam get&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; get parameter<br>
&nbsp;&nbsp;&nbsp; rosparam load&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; load parameters from file<br>
&nbsp;&nbsp;&nbsp; rosparam dump&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; dump parameters to file<br>
&nbsp;&nbsp;&nbsp; rosparam delete&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; delete parameter<br>
&nbsp;&nbsp;&nbsp; rosparam list&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; list parameter names<br>
<br>
&nbsp;&nbsp;&nbsp; list<br>
&nbsp;&nbsp;&nbsp; set/get<br>
&nbsp;&nbsp;&nbsp; dump/load<br>
<p>&nbsp;&nbsp;&nbsp; <br>
</p>
<p>Stack, Package, Nodes/Messages/Services/Libraries/Tools， 关系复习图如下：<br>
</p>
<br>
<img src="http://img.blog.csdn.net/20150707122239373?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" height="422" width="449"><br>
<p></p>
   

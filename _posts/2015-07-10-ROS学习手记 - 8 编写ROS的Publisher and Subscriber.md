---
layout: post
title: 'ROS学习手记 - 8 编写ROS的Publisher and Subscriber'
date: 2015-07-10 06:52:00 +0800
category: from_cnblogs
slug: p20150710065200
---


<p><br>
</p>
<p>上一节我们完成了 message &amp; srv 文件的创建和加入编译，这次我们要玩简单的Publisher 和 Subscriber</p>
<p>要玩 Publisher 和 Subscriber， 需要具备的条件有哪些呢？先总结一下：</p>
<ol>
<li>&nbsp;创建并生成自己的Package，本次是 beginner_tutorials</li><li>&nbsp;创建并生成ROS message &amp; srv</li></ol>
<p>详细版的手记要看我上传到百度文库的文件了：http://wenku.baidu.com/view/f872755a26fff705cc170ade<br>
这里会写个总结~</p>
<ol>
<li>
<h2><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29">Writing a Simple Publisher and Subscriber (C&#43;&#43;)</a></h2>
<p><span class="anchor" id="line-1-11"></span>This tutorial covers how to write a publisher and subscriber node in C&#43;&#43;.</p>
</li></ol>
<p align="center"><br>
</p>
<h2 align="center"><strong>ROS学习笔记10 - 编写编译和检验Service Node<br>
</strong></h2>
<p><strong>百度文库：http://wenku.baidu.com/view/e14d5e18dd88d0d232d46a37</strong></p>
<strong></strong>
<p align="left"><strong>Writing a Service Node</strong></p>
<p align="left">Here we'll create the service (&quot;add_two_ints_server&quot;)node which will receive two ints and return the sum.</p>
<p align="left">Change directories to your beginner_tutorialspackage you created in your catkin workspace previous tutorials:</p>
<p>cd ~/catkin_ws/src/beginner_tutorials</p>
<p align="left">Please make sure you have followed the directions in the previous tutorialfor creating the service needed in this tutorial,<a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv#Creating_a_srv">creating
 the AddTwoInts.srv</a> (be sure to choose the rightversion of build tool you're using at the top of wiki page in the link).</p>
<p align="left"><strong>The Code</strong></p>
<p align="left">Create the src/add_two_ints_server.cpp file within the <span style="color:#1F497D">
beginner_tutorials</span>package and paste the following inside it: </p>
<p align="left"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">切换行号显示</a></p>
<div>
<p>#include &quot;ros/ros.h&quot;</p>
<p>#include &quot;beginner_tutorials/AddTwoInts.h&quot;</p>
<p>&nbsp;</p>
<p>bool add(beginner_tutorials::AddTwoInts::Request&nbsp; &amp;req,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;beginner_tutorials::AddTwoInts::Response &amp;res)</p>
<p>{</p>
<p>&nbsp; res.sum = req.a &#43; req.b;</p>
<p>&nbsp; ROS_INFO(&quot;request:x=%ld, y=%ld&quot;, (long int)req.a, (long int)req.b);</p>
<p>&nbsp; ROS_INFO(&quot;sendingback response: [%ld]&quot;, (long int)res.sum);</p>
<p>&nbsp; return true;</p>
<p>}</p>
<p>&nbsp;</p>
<p>int main(int argc, char **argv)</p>
<p>{</p>
<p>&nbsp; ros::init(argc, argv, &quot;add_two_ints_server&quot;);</p>
<p>&nbsp; ros::NodeHandle n;</p>
<p>&nbsp;</p>
<p>&nbsp; ros::ServiceServer service = n.advertiseService(&quot;add_two_ints&quot;,add);</p>
<p>&nbsp; ROS_INFO(&quot;Readyto add two ints.&quot;);</p>
<p>&nbsp; ros::spin();</p>
<p>&nbsp;</p>
<p>&nbsp; return 0;</p>
<p>}</p>
</div>
<p align="left"><strong>The Code Explained</strong></p>
<p align="left">Now, let's break the code down. </p>
<p align="left"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">切换行号显示</a></p>
<div>
<p>#include &quot;ros/ros.h&quot;</p>
<p>#include &quot;beginner_tutorials/AddTwoInts.h&quot;</p>
</div>
<p align="left">&nbsp;</p>
<p align="left">beginner_tutorials/AddTwoInts.h is the header file generated fromthe srv file that we created earlier. 位置：~/catkin_ws/include/beginner_tutorials</p>
<p align="left"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">切换行号显示</a></p>
<div>
<p>bool add(beginner_tutorials::AddTwoInts::Request&nbsp;&amp;req,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;beginner_tutorials::AddTwoInts::Response &amp;res)</p>
</div>
<p align="left">This function provides the service for adding two ints,it takes in the request and response type defined in the srvfile(AddTwoInts.srv) and returns a boolean.</p>
<p align="left"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">切换行号显示</a></p>
<div>
<p>{</p>
<p>&nbsp; res.sum = req.a &#43; req.b;</p>
<p>&nbsp; ROS_INFO(&quot;request:x=%ld, y=%ld&quot;, (long int)req.a, (long int)req.b);</p>
<p>&nbsp; ROS_INFO(&quot;sendingback response: [%ld]&quot;, (long int)res.sum);</p>
<p>&nbsp; return true;</p>
<p>}</p>
</div>
<p align="left">Here the two ints are added and stored in theresponse. Then some information about the request and response are logged.Finally the service returns true when it is complete.</p>
<p align="left"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">切换行号显示</a></p>
<div>
<p>ros::ServiceServerservice = n.advertiseService(&quot;add_two_ints&quot;, add);</p>
</div>
<p align="left">Here the service is created and advertised over ROS. <br>
对比一下前一节publisher nodes的创建：</p>
<div>
<p><span style="color:gray">&nbsp;ros::Publisher&nbsp;chatter_pub&nbsp;=&nbsp;n.advertise&lt;std_msgs::String&gt;(</span>&quot;chatter&quot;,&nbsp;1000);&nbsp; //topic name “chatter”, bufferSize:1000</p>
</div>
<p align="left">对比一下后面client相关声明语句：</p>
<div>
<p><span style="color:gray">ros</span><span style="color:gray">::ServiceClient</span><span style="color:gray"> client = n.serviceClient</span>&lt;beginner_tutorials::AddTwoInts&gt;(&quot;add_two_ints&quot;);</p>
</div>
<p align="left">This creates a client for the add_two_ints service. The ros::ServiceClient object is used to call the service later on.</p>
<p align="left"><strong>&nbsp;</strong></p>
<p align="left"><strong>Writing the Client Node</strong></p>
<p align="left"><strong>The Code</strong></p>
<p align="left">Create the src/add_two_ints_client.cpp file within the <span style="color:#1F497D">
beginner_tutorials</span>package and paste the following inside it: </p>
<p align="left"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">切换行号显示</a></p>
<div>
<p>#include &quot;ros/ros.h&quot;</p>
<p>#include &quot;beginner_tutorials/AddTwoInts.h&quot;</p>
<p>#include &lt;cstdlib&gt;</p>
<p>&nbsp;</p>
<p>int main(int argc, char **argv)</p>
<p>{</p>
<p>&nbsp; ros::init(argc, argv, &quot;add_two_ints_client&quot;);</p>
<p>&nbsp; if (argc != 3)</p>
<p>&nbsp; {</p>
<p>&nbsp;&nbsp;&nbsp; ROS_INFO(&quot;usage:add_two_ints_client X Y&quot;);</p>
<p>&nbsp;&nbsp;&nbsp; return1;</p>
<p>&nbsp; }</p>
<p>&nbsp;</p>
<p>&nbsp; ros::NodeHandle n;</p>
<p>&nbsp; ros::ServiceClient client = n.serviceClient&lt;beginner_tutorials::AddTwoInts&gt;(&quot;add_two_ints&quot;);</p>
<p>&nbsp; beginner_tutorials::AddTwoInts srv;</p>
<p>&nbsp; srv.request.a= atoll(argv[1]);</p>
<p>&nbsp; srv.request.b= atoll(argv[2]);</p>
<p>&nbsp; if (client.call(srv))</p>
<p>&nbsp; {</p>
<p>&nbsp;&nbsp;&nbsp; ROS_INFO(&quot;Sum:%ld&quot;, (long int)srv.response.sum);</p>
<p>&nbsp; }</p>
<p>&nbsp; else</p>
<p>&nbsp; {</p>
<p>&nbsp;&nbsp;&nbsp; ROS_ERROR(&quot;Failedto call service add_two_ints&quot;);</p>
<p>&nbsp;&nbsp;&nbsp; return1;</p>
<p>&nbsp; }</p>
<p>&nbsp;</p>
<p>&nbsp; return 0;</p>
<p>}</p>
</div>
<p align="left"><strong>The Code Explained</strong></p>
<p align="left">Now, let's break the code down. </p>
<p align="left"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">切换行号显示</a></p>
<div>
<p>&nbsp; ros::ServiceClient client = n.serviceClient&lt;beginner_tutorials::AddTwoInts&gt;(&quot;add_two_ints&quot;);</p>
</div>
<p>This creates a client for the add_two_intsservice. <span style="color:red">The ros</span>::ServiceClient object is used to call the service later on.</p>
<p align="left"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">切换行号显示</a></p>
<div>
<p>&nbsp; beginner_tutorials::AddTwoInts srv;</p>
<p>&nbsp; srv.request.a= atoll(argv[1]);</p>
<p>&nbsp; srv.request.b= atoll(argv[2]);</p>
</div>
<p align="left">Here we instantiate列举 an autogenerated service class, and assignvalues into its request member.<span style="color:#1F497D">Aservice class contains two members</span>,
<strong>requestand response</strong>.<span style="color:#1F497D">It alsocontains two class definitions</span>,
<strong>Requestand Response</strong>. </p>
<p align="left"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">切换行号显示</a></p>
<div>
<p>&nbsp; if (client.call(srv))</p>
</div>
<p align="left">This actually calls the service. Since service calls are blocking, it willreturn once the call is done. If the service call succeeded, call()will return true and the value in srv.response willbe valid. If the call did not succeed, call() willreturn
 false and the value in srv.response will beinvalid. </p>
<p align="left"><strong>Building your nodes</strong></p>
<p align="left">Again edit the beginner_tutorials CMakeLists.txt located at ~/catkin_ws/src/beginner_tutorials/CMakeLists.txt and add the following at the end:</p>
<p align="left"><a target="_blank" target="_blank" href="https://raw.github.com/ros/catkin_tutorials/master/create_package_srvclient/catkin_ws/src/beginner_tutorials/CMakeLists.txt"><em>https://raw.github.com/ros/catkin_tutorials/master/create_package_srvclient/catkin_ws/src/beginner_tutorials/CMakeLists.txt</em></a></p>
<p align="left"><a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28c%2B%2B%29">切换行号显示</a></p>
<div>
<p><a target="_blank" target="_blank" name="OLE_LINK2"></a><a target="_blank" target="_blank" name="OLE_LINK1"></a>add_executable(add_two_ints_server src/add_two_ints_server.cpp)</p>
<p>target_link_libraries(add_two_ints_server ${catkin_LIBRARIES})</p>
<p>add_dependencies(add_two_ints_server beginner_tutorials_gencpp)</p>
<p>&nbsp;</p>
<p>add_executable(add_two_ints_client src/add_two_ints_client.cpp)</p>
<p>target_link_libraries(add_two_ints_client ${catkin_LIBRARIES})</p>
<p>add_dependencies(add_two_ints_client beginner_tutorials_gencpp)</p>
</div>
<p align="left">This will create two executables, add_two_ints_server and add_two_ints_client, which by default will go intopackage directory of your<a target="_blank" target="_blank" href="http://wiki.ros.org/catkin/workspaces#Development_.28Devel.29_Space">devel
 space</a>, located by default at ~/catkin_ws/devel/lib/share/&lt;package&nbsp;name&gt;. You can invoke executablesdirectly or you can use rosrun to invoke调用 them. They are not placed in '&lt;prefix&gt;/bin' because that would pollute thePATH when installing your package to
 the system. If you wish for your executableto be on the PATH at installation time, you can setup an install target, see:<a target="_blank" target="_blank" href="http://wiki.ros.org/catkin/CMakeLists.txt">catkin/CMakeLists.txt</a></p>
<p align="left">For more detailed discription of the <a target="_blank" target="_blank" href="http://wiki.ros.org/catkin/CMakeLists.txt">
CMakeLists.txt</a> file see: <a target="_blank" target="_blank" href="http://wiki.ros.org/catkin/CMakeLists.txt">
catkin/CMakeLists.txt</a> </p>
<p align="left">Now run catkin_make: </p>
<p># In your catkin workspace</p>
<p>cd ~/catkin_ws</p>
<p>catkin_make</p>
<p align="left">If your build fails for some reason: </p>
<ul type="disc">
<li>make sure you have followed the directions in the previous tutorial: <a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv#Creating_a_srv">
creating the AddTwoInts.srv</a>. </li></ul>
<p align="left">Now that you have written a simple service and client, let's <a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/ExaminingServiceClient">
examine the simple service and client</a>. </p>
<h2>Examining the Simple Service and Client</h2>
<h3>Running the Service</h3>
<p>Let's start by running the service: </p>
<p>$ rosrunbeginner_tutorials add_two_ints_server&nbsp;&nbsp;&nbsp;&nbsp; (C&#43;&#43;)</p>
<p>$ rosrunbeginner_tutorials add_two_ints_server.py&nbsp; (Python) </p>
<p>You should see something similar to: </p>
<p>Ready to add two ints.</p>
<h3>Running the Client</h3>
<p>Now let's run the client with the necessaryarguments: </p>
<p>$ rosrunbeginner_tutorials add_two_ints_client1 3&nbsp;&nbsp;&nbsp;&nbsp; (C&#43;&#43;)</p>
<p>$ rosrunbeginner_tutorials add_two_ints_client.py 1 3&nbsp; (Python) </p>
<p>You should see something similar to: </p>
<p>Requesting 1&#43;3</p>
<p>1 &#43; 3 = 4</p>
<p>Now that you've successfully run your first serverand client, let's learn how to<a target="_blank" target="_blank" href="http://wiki.ros.org/ROS/Tutorials/Recording%20and%20playing%20back%20data">record and play back data</a>.
</p>
<h3>Further examples on Service and Client nodes</h3>
<p>If you want to investigate further and get ahands-on example, you can get one <a target="_blank" target="_blank" href="https://github.com/fairlight1337/ros_service_examples/">
here</a>. A simple Client and Servicecombination shows the use of custom message types. The Service node is writtenin C&#43;&#43; while the Client is available in C&#43;&#43;, Python and LISP.</p>
<br>
<p></p>
<p><br>
</p>
<p><br>
</p>
   

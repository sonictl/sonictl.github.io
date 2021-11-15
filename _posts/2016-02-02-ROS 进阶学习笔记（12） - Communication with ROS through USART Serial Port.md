---
layout: post
title: 'ROS 进阶学习笔记（12） - Communication with ROS through USART Serial Port'
date: 2016-02-02 06:24:00 +0800
category: from_cnblogs
slug: p20160202062400
---


<h1>Communication with ROS through USART Serial Port<br>
</h1>
<p>We always need to communicate with ROS through serial port as we have many devices like sensors, touch-screen, actuators to be controlled through USART serial protocol.</p>
<p>After some investigation, I found several ways that can make ROS work with the serial-port-devices:</p>
<ul>
<li>Use the package: <a target="_blank" target="_blank" href="http://wiki.ros.org/rosserial/">
rosserial</a>. It seems like only the &quot;connected rosserial-enabled device&quot; can work upon that, including Aduino, Windows, XBee, etc.(<a target="_blank" target="_blank" href="http://answers.ros.org/question/225495/how-to-use-rosserial_python/?answer=225611#post-id-225611">ROS
 community</a> said: rosserial is used with mcus that have rosserial code running on them.)<br>
</li><li>Use the driver package: <a target="_blank" target="_blank" href="http://wiki.ros.org/serial/">
serial</a>. It seems like a serial port driver for ROS programming. This should be the most free way but I didn't find out how to use it.</li><li>Use the package: <a target="_blank" target="_blank" href="http://wiki.ros.org/cereal_port">
cereal_port</a>, a serial port library for ROS, you can find it in the<a target="_blank" target="_blank" href="http://www.ros.org/wiki/serial_communication/">serial_communication stack</a>, it's easy to use.</li><li>The package:<a target="_blank" target="_blank" href="http://www.ros.org/wiki/rosbridge"> rosbridge</a><em>may</em> also suit your needs (full disclosure, author of rosbridge here). Check out<a target="_blank" target="_blank" href="http://www.youtube.com/watch?v=lo2diadVEG4">this
 video</a> of using rosbridge with an Arduino (though the principles apply to any uController).<br>
</li><li>A simple approach is to write a ROS node that can read/write text messages exchanged with the microcontroller over a serial port and convert these serial messages to ROS style std_msgs/String messages.<strong>Here we are going to share this way first.<br>
</strong></li><li>Kevin: a <a target="_blank" target="_blank" href="https://github.com/walchko/serial_node">
service</a> given by Kevin. It seems to work pretty good. This works well cuz a lot of our serial stuff is sending a message and receiving a response back, instead of separate publish/subscribes.<a target="_blank" target="_blank" href="http://answers.ros.org/question/10114/how-can-ros-communicate-with-my-microcontroller/?sort=latest#sort-top">reference
 web</a><br>
</li><li><br>
</li></ul>
<p>A simple approach is to write a ROS node that can read/write text messages exchanged with the microcontroller over a serial port and convert these serial messages to ROS style std_msgs/String messages. Here we are going to share this way first. ref:<a target="_blank" target="_blank" href="http://answers.ros.org/question/10114/how-can-ros-communicate-with-my-microcontroller/?sort=latest#sort-top">reference
 web</a>&nbsp;</p>
<p>In that page, Bart says:<br>
<strong>&quot;A simple approach is to write a ROS node that can read/write text messages exchanged with the microcontroller over a serial port and convert these serial messages to ROS style std_msgs/String messages. A separate ROS node can then be written to convert
 the simple text messages to specific ROS message formats (cmd_vel, odom, etc) based on whatever message protocol was defined for the exchange with the microcontroller.</strong></p>
<p><strong>Attached below is the source for code that I used for this purpose. There is nothing original in the code, but it may be helpful to someone new to ROS and Unix/Linux serial communication. Posting this type of a listing may be a bit unconventional
 for this site as short answers are generally preferred. Hopefully no one is offended. I would be interested in hearing if there are other successful approaches to the problem.&quot;</strong></p>
<h3>Let's implement this approach:</h3>
<p>Enter the catkin workspace, build the package: (<a target="_blank" target="_blank" href="http://wiki.ros.org/catkin/Tutorials/CreatingPackage">tutorial page</a>)<br>
</p>
<pre><span style="font-family:Courier New">$ cd ~/catkin_ws/src
$ catkin_create_pkg r2serial_driver std_msgs rospy roscpp
$ cd ~/catkin_ws
$ catkin_make</span>
</pre>
<p></p>
<p class="line874">To add the workspace to your ROS environment you need to source the generated setup file:<span class="anchor" id="ROS.2BAC8-Tutorials.2BAC8-catkin.2BAC8-CreatingPackage.line-77"></span><span class="anchor" id="ROS.2BAC8-Tutorials.2BAC8-catkin.2BAC8-CreatingPackage.line-78"></span></p>
<p class="line867"><span class="anchor" id="ROS.2BAC8-Tutorials.2BAC8-catkin.2BAC8-CreatingPackage.line-79"></span><span class="anchor" id="ROS.2BAC8-Tutorials.2BAC8-catkin.2BAC8-CreatingPackage.line-80"></span></p>
<pre><span class="anchor" id="ROS.2BAC8-Tutorials.2BAC8-catkin.2BAC8-CreatingPackage.line-1-7"></span><span style="font-family:Courier New">$ . ~/catkin_ws/devel/setup.bash
$ cd ~/catkin_ws/src/r2serial_driver/src</span></pre>
<p>Add a file: &quot;r2serial.cpp&quot;, Source code:<br>
</p>
<p></p>
<pre code_snippet_id="1572730" snippet_file_name="blog_20160202_1_6250658" name="code" class="cpp">/*                                                                                                                       
 * Copyright (c) 2010, Willow Garage, Inc.                                                                               
 * All rights reserved.                                                                                                  
 *                                                                                                                       
 * Redistribution and use in source and binary forms, with or without                                                    
 * modification, are permitted provided that the following conditions are met:                                           
 *                                                                                                                       
 *     * Redistributions of source code must retain the above copyright                                                  
 *       notice, this list of conditions and the following disclaimer.                                                   
 *     * Redistributions in binary form must reproduce the above copyright                                               
 *       notice, this list of conditions and the following disclaimer in the                                             
 *       documentation and/or other materials provided with the distribution.                                            
 *     * Neither the name of the Willow Garage, Inc. nor the names of its                                                
 *       contributors may be used to endorse or promote products derived from                                            
 *       this software without specific prior written permission.                                                        
 *                                                                                                                       
 * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS &quot;AS IS&quot;                                           
 * AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE                                             
 * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE                                            
 * ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE                                              
 * LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR                                                   
 * CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF                                                  
 * SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS                                              
 * INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN                                               
 * CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)                                               
 * ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE                                            
 * POSSIBILITY OF SUCH DAMAGE.                                                                                           
 */

//r2serial.cpp

// communicate via RS232 serial with a remote uController. 
// communicate with ROS using String type messages.
// subscribe to command messages from ROS
// publish command responses to ROS

// program parameters - ucontroller# (0,1), serial port, baud rate

//Thread main
//  Subscribe to ROS String messages and send as commands to uController
//Thread receive
//  Wait for responses from uController and publish as a ROS messages


#include &quot;ros/ros.h&quot;
#include &quot;std_msgs/String.h&quot;
#include &lt;sstream&gt;
#include &lt;pthread.h&gt;
#include &lt;sys/types.h&gt;
#include &lt;sys/stat.h&gt;
#include &lt;sys/time.h&gt;
#include &lt;fcntl.h&gt;
#include &lt;termios.h&gt;
#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;

#define DEFAULT_BAUDRATE 19200
#define DEFAULT_SERIALPORT &quot;/dev/ttyUSB0&quot;

//Global data
FILE *fpSerial = NULL;   //serial port file pointer
ros::Publisher ucResponseMsg;
ros::Subscriber ucCommandMsg;
int ucIndex;          //ucontroller index number


//Initialize serial port, return file descriptor
FILE *serialInit(char * port, int baud)
{
  int BAUD = 0;
  int fd = -1;
  struct termios newtio;
  FILE *fp = NULL;

 //Open the serial port as a file descriptor for low level configuration
 // read/write, not controlling terminal for process,
  fd = open(port, O_RDWR | O_NOCTTY | O_NDELAY );
  if ( fd&lt;0 )
  {
    ROS_ERROR(&quot;serialInit: Could not open serial device %s&quot;,port);
    return fp;
  }

  // set up new settings
  memset(&amp;newtio, 0,sizeof(newtio));
  newtio.c_cflag =  CS8 | CLOCAL | CREAD;  //no parity, 1 stop bit

  newtio.c_iflag = IGNCR;    //ignore CR, other options off
  newtio.c_iflag |= IGNBRK;  //ignore break condition

  newtio.c_oflag = 0;        //all options off

  newtio.c_lflag = ICANON;     //process input as lines

  // activate new settings
  tcflush(fd, TCIFLUSH);
  //Look up appropriate baud rate constant
  switch (baud)
  {
     case 38400:
     default:
        BAUD = B38400;
        break;
     case 19200:
        BAUD  = B19200;
        break;
     case 9600:
        BAUD  = B9600;
        break;
     case 4800:
        BAUD  = B4800;
        break;
     case 2400:
        BAUD  = B2400;
        break;
     case 1800:
        BAUD  = B1800;
        break;
     case 1200:
        BAUD  = B1200;
        break;
  }  //end of switch baud_rate
  if (cfsetispeed(&amp;newtio, BAUD) &lt; 0 || cfsetospeed(&amp;newtio, BAUD) &lt; 0)
  {
    ROS_ERROR(&quot;serialInit: Failed to set serial baud rate: %d&quot;, baud);
    close(fd);
    return NULL;
  }
  tcsetattr(fd, TCSANOW, &amp;newtio);
  tcflush(fd, TCIOFLUSH);

  //Open file as a standard I/O stream
  fp = fdopen(fd, &quot;r+&quot;);
  if (!fp) {
    ROS_ERROR(&quot;serialInit: Failed to open serial stream %s&quot;, port);
    fp = NULL;
  }
  return fp;
} //serialInit


//Process ROS command message, send to uController
void ucCommandCallback(const std_msgs::String::ConstPtr&amp; msg)
{
  ROS_DEBUG(&quot;uc%dCommand: %s&quot;, ucIndex, msg-&gt;data.c_str());
  fprintf(fpSerial, &quot;%s&quot;, msg-&gt;data.c_str()); //appends newline
} //ucCommandCallback


//Receive command responses from robot uController
//and publish as a ROS message
void *rcvThread(void *arg)
{
  int rcvBufSize = 200;
  char ucResponse[rcvBufSize];   //response string from uController
  char *bufPos;
  std_msgs::String msg;
  std::stringstream ss;

  ROS_INFO(&quot;rcvThread: receive thread running&quot;);

  while (ros::ok()) {
    bufPos = fgets(ucResponse, rcvBufSize, fpSerial);
    if (bufPos != NULL) {
      ROS_DEBUG(&quot;uc%dResponse: %s&quot;, ucIndex, ucResponse);
      msg.data = ucResponse;
      ucResponseMsg.publish(msg);
    }
  }
  return NULL;
} //rcvThread


int main(int argc, char **argv)
{
  char port[20];    //port name
  int baud;     //baud rate 

  char topicSubscribe[20];
  char topicPublish[20];

  pthread_t rcvThrID;   //receive thread ID
  int err;

  //Initialize ROS
  ros::init(argc, argv, &quot;r2serial_driver&quot;);
  ros::NodeHandle rosNode;
  ROS_INFO(&quot;r2serial_driver starting&quot;);

  //Open and initialize the serial port to the uController
  if (argc &gt; 1) {
    if(sscanf(argv[1],&quot;%d&quot;, &amp;ucIndex)==1) {
      sprintf(topicSubscribe, &quot;uc%dCommand&quot;,ucIndex);
      sprintf(topicPublish, &quot;uc%dResponse&quot;,ucIndex);
    }
    else {
      ROS_ERROR(&quot;ucontroller index parameter invalid&quot;);
      return 1;
    }
  }
  else {
    strcpy(topicSubscribe, &quot;uc0Command&quot;);
    strcpy(topicPublish, &quot;uc0Response&quot;);
  }

  strcpy(port, DEFAULT_SERIALPORT);
  if (argc &gt; 2)
     strcpy(port, argv[2]);

  baud = DEFAULT_BAUDRATE;
  if (argc &gt; 3) {
    if(sscanf(argv[3],&quot;%d&quot;, &amp;baud)!=1) {
      ROS_ERROR(&quot;ucontroller baud rate parameter invalid&quot;);
      return 1;
    }
  }

  ROS_INFO(&quot;connection initializing (%s) at %d baud&quot;, port, baud);
   fpSerial = serialInit(port, baud);
  if (!fpSerial )
  {
    ROS_ERROR(&quot;unable to create a new serial port&quot;);
    return 1;
  }
  ROS_INFO(&quot;serial connection successful&quot;);

  //Subscribe to ROS messages
  ucCommandMsg = rosNode.subscribe(topicSubscribe, 100, ucCommandCallback);

  //Setup to publish ROS messages
  ucResponseMsg = rosNode.advertise&lt;std_msgs::String&gt;(topicPublish, 100);

  //Create receive thread
  err = pthread_create(&amp;rcvThrID, NULL, rcvThread, NULL);
  if (err != 0) {
    ROS_ERROR(&quot;unable to create receive thread&quot;);
    return 1;
  }

  //Process ROS messages and send serial commands to uController
  ros::spin();

  fclose(fpSerial);
  ROS_INFO(&quot;r2Serial stopping&quot;);
  return 0;
}</pre>
<p>Modify the CMakeList.txt file:<br>
</p>
<pre><span style="font-family:Courier New">$ cd ~/catkin_ws/src/r2serial_driver
$ gedit CMakeList.txt
</span></pre>
<p>add/modify these lines as below:</p>
<p></p>
<pre code_snippet_id="1572730" snippet_file_name="blog_20160202_2_9565708" name="code" class="plain">find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_executable(r2serial_driver src/r2serial.cpp)

add_dependencies(r2serial_driver ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(r2serial_driver
   ${catkin_LIBRARIES}
 )

</pre>Save and Build the package again:<br>
<p></p>
<pre><span style="font-family:Courier New">$ cd ~/catkin_ws</span>
<span style="font-family:Courier New">$ catkin_make -DCMAKE_BUILD_TYPE=Release </span></pre>
<p></p>
<p>He also offered a simple launch file that interfaces to two microcontrollers using two serial connections, put the code into<br>
&nbsp; &quot;~/catkin_ws/src/r2serial_driver/launch/r2serial_driver.launch&quot; file:<br>
</p>
<p></p>
<pre code_snippet_id="1572730" snippet_file_name="blog_20160202_3_8311972" name="code" class="html">&lt;launch&gt;
  &lt;node pkg=&quot;r2serial_driver&quot; type=&quot;r2serial_driver&quot; name=&quot;r2serial0&quot; args=&quot;0 /dev/ttyUSB0 9600&quot; output=&quot;screen&quot; &gt;
  &lt;remap from=&quot;ucCommand&quot; to=&quot;uc0Command&quot; /&gt;
  &lt;remap from=&quot;ucResponse&quot; to=&quot;uc0Response&quot; /&gt;
  &lt;/node&gt;

  &lt;node pkg=&quot;r2serial_driver&quot; type=&quot;r2serial_driver&quot; name=&quot;r2serial1&quot; args=&quot;1 /dev/ttyUSB1 9600&quot; output=&quot;screen&quot; &gt;
  &lt;remap from=&quot;ucCommand&quot; to=&quot;uc1Command&quot; /&gt;
  &lt;remap from=&quot;ucResponse&quot; to=&quot;uc1Response&quot; /&gt;
  &lt;/node&gt;

&lt;/launch&gt;</pre>
<p>Usage:</p>
<pre><span style="font-family:Courier New">$ rosrun r2serial_driver r2serial_driver 0 /dev/ttyUSB0 9600</span>
</pre>
<p>or</p>
<pre><span style="font-family:Courier New">$ roslaunch r2serial_driver r2serial_driver.launch</span>
</pre>
<p>try:<br>
</p>
<pre><span style="font-family:Courier New">$ rostopic pub -r 1 /uc0Command std_msgs/String hello_my_serial
$ rostopic echo /uc0Response</span></pre>
You may also need to remap the /dev/ttyUSB* to some name you like cuz you may have several usb-serial devices.<br>
If so, just Ref:<span style="font-size:12px"><span class="link_title"><a target="_blank" target="_blank" href="http://blog.csdn.net/sonictl/article/details/50947753">重新指派usb转串口模块在linux系统中的设备调用名称(English Version: remap your usb-serial devices' names in Linux
 )</a></span></span><br>
<br>
<p>But <strong>Kevin</strong> mentioned a question: &quot;Interesting, but a lot of my serial stuff is send a message and receive a response back. Instead of separate publish/subscribes have you tried a service? That seems like it would work nicely for this.&quot;</p>
<p>I also noticed a problem: <u><span style="color:#CC0000">The Linux's System RAM and CPU will be much costed by r2serial_driver when it is running.</span></u><br>
</p>
<p>Next time, we'll see how to use <a target="_blank" target="_blank" href="https://github.com/walchko/serial_node">
Kevin's service</a> to play ROS serial communication.<br>
</p>
<h2>2. Use Service to play ROS-Serial communication</h2>
<p>see this blog brother below...</p>
<p><a target="_blank" target="_blank" href="http://blog.csdn.net/sonictl/article/details/51372534">http://blog.csdn.net/sonictl/article/details/51372534</a><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p></p>
<p><br>
</p>
   

---
layout: post
title: 'ROS Industrial 简介'
date: 2015-07-25 15:28:00 +0800
category: from_cnblogs
slug: p20150725152800
---


<p>ROS_I means ROS Industrial<br>
</p>
<p><br>
</p>
<p>ROS_I 解决了哪些问题：</p>
&nbsp; 1. 让自动化可以互相协作，操纵器、末端执行器、感知系统/传感器，移动平台，周边设备，都可只用一种语言（ROS messages）交流。在无视OEM厂商品牌或者通信总线的情况下，也能协作。<br>
&nbsp; 2. 免费的封装模块库，提供了高级能力，最好的技术可以得到传播，绕开了专有厂商的锁定，再培训或价&#26684;问题。<br>
&nbsp; 3. 为制造自动化软件的开源开发，提供了监管和结构。在这个项目中的代码都是被复审过，并且自动评估质量指标。<br>
&nbsp; 4. 为学术研究提供了一个自我审查的渠道，使之可以快速应用到工业中。<br>
<br>
Can I develop closed-source software for ROS-I and sell it for profit?<br>
A: Yes! We desire for the ROS-I community to foster all manner of new businesses selling ROS-I compatible software nodes, development tools, GUIs, support and integration services.<br>
<br>
Is ROS-Industrial suitable for real-time control?<br>
<br>
A: Like ROS, ROS-I nominally runs on Ubuntu Linux, which is not a real-time OS. ROS-I is fast enough to run closed-loop with perception systems for manufacturing automation applications, but (at least for now) ROS-I must be used as a high-level controller in
 conjunction with a low-level real-time controller (usually the one from the OEM), which closes servo feedback loops and provides safety behavior (e.g. an E-stop).
   

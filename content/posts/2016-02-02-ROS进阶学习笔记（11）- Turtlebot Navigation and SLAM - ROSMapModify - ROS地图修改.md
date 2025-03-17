---
layout: post
title: 'ROS进阶学习笔记（11）- Turtlebot Navigation and SLAM - ROSMapModify - ROS地图修改'
date: 2016-02-02 05:57:00 +0800
category: from_cnblogs
slug: p20160202055700
---
<h1>ROS进阶学习笔记（11）- Turtlebot Navigation and SLAM - 2 - MapModify地图修改</h1>
<p>We can use gmapping model to generate the map file: **.pgm and **.amcl, the latter is just a refer to the **.pgm map file.</p>
<p>Here I introduce how to use the image editor "" to modify the **.pgm file to meet our requirment.</p>
<p>We just used <strong><span style="color: #cc0000;">gimp</span></strong>. It's just a bitmap, so we drew some extra black on there with the paintbrush. You may think that gimp is too big size.&darr;<br />
</p>
<p><a href="https://askubuntu.com/questions/845394/light-weight-image-editor-that-can-support-pgm-format-in-linux/845543#845543" target="_blank">Here is some substitutions of the big-sized pgm file editor gimp:</a></p>
<div class="post-text">
<p><strong>Krita</strong></p>
<p>It's in universe ubuntu repos and is lighter than gimp. I recommend.</p>
<div class="post-text">
<h3>Inkscape</h3>
<p>Inkscape which seems to support .pgm extension (disclaimer: I haven't tested it). Install it with</p>
<pre><code>sudo apt install inkscape
</code></pre>
<h3>Netpbm</h3>
<p><a href="http://fileinfo.com/extension/pgm" rel="nofollow noreferrer">This page</a> reports that <code>netpbm</code> can be a cli solution for *.pgm extensions too. Try it. This is the <a href="http://netpbm.sourceforge.net/" rel="nofollow noreferrer">homepage of Netpbm</a>.</p>
</div>
</div>
<p><br />
</p>
<p>&nbsp;</p>
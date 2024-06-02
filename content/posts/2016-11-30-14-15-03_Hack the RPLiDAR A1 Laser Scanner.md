---
layout: post
title:  "Hack the RPLiDAR A1 Laser Scanner"
date: 2016-11-30 14:15:03 +0800
tags: 
slug: p20161130141503
---

# Hack the RPLiDAR A1 Laser Scanner





## Hack the RPLiDAR-A1 Laser [Scanner](https://so.csdn.net/so/search?q=Scanner&spm=1001.2101.3001.7020)


Hacking the RPLiDAR A1 is very similar with the Hacking of Neato XV-11 Laser Scanner:  
 


<http://wiki.ros.org/xv_11_laser_driver/Tutorials/Connecting%20the%20XV-11%20Laser%20to%20USB>


  
 


They all use UART-TTL as the data transmission protocal. So we need to find out the TX and RX wire for the scanner to communicate with the UART-USB converter.


Below is the picture of the RP-LiDAR A1:


![](http://www.robotshop.com/media/files/images2/rplidar-360-laser-scanner-7-large.jpg)


It consists of a driving motor, a laser scanner, a circuit board and a converting board.  
 


### Equivalent Circuit for RPLiDAR A1


![](https://img-blog.csdn.net/20161206105523988?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


### Resources


You can also Download the RP\_Lidar assessment software Here:


official website: <http://www.slamtec.com/en/lidar/a1>


ROS wiki site: <https://github.com/robopeak/rplidar_ros>  
 


  
 


### Replace the slip ring by Bluetooth Transceiver


The slip ring which was originally designed as the data and power transmitter is not durable. The data became not continuous after some days' use.  
 In order to transfer the data with a revolving scanner, I choose to use the wireless solution: bluetooth, XBee, photoelectricity etc.  
 


Finally, I added a bluetooth transceiver on the top of the scanner:


![](https://img-blog.csdn.net/20161130140725272?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


  
 


![](https://img-blog.csdn.net/20161130141843122?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


  
 


![](https://img-blog.csdn.net/20161130140746866?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


  
 


  
 


![](https://img-blog.csdn.net/20161130140759803?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


  
 


![](https://img-blog.csdn.net/20161130141236766?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


  
 


![](https://img-blog.csdn.net/20161130141441362?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


  
 


### What's More


Contains the Neato XV-11, there is another corporation selling laser scanner based robot:<http://news.mydrivers.com/1/501/501871_all.htm>  
 


![](http://img1.mydrivers.com/img/20161001/S27426e21-4c6a-4f31-ade8-5684fd7ea79c.jpg)


![](http://img1.mydrivers.com/img/20161001/Sdf606da1-b482-4ebf-8af4-59b651f36c38.jpg)  
 





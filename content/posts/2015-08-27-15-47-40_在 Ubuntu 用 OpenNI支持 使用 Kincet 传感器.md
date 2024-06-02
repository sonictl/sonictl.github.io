---
layout: post
title:  "在 Ubuntu 用 OpenNI支持 使用 Kincet 传感器"
date: 2015-08-27 15:47:40 +0800
tags: 
slug: p20150827154740
---

# 在 Ubuntu 用 OpenNI支持 使用 Kincet 传感器





## 


### Use [Kinect](https://so.csdn.net/so/search?q=Kinect&spm=1001.2101.3001.7020) on Ubuntu with OpenNI（中文在后）


You have to Refer  [this blog](http://www.20papercups.net/programming/kinect-on-ubuntu-with-openni/) ([backup link](http://blog.csdn.net/sonictl/article/details/47974485#t10))and you may meet some incompatible contents, do not worry, the thought and path is correct.


The very important doc you should read carefully is that on  [OpenNI's GitHub site](https://github.com/OpenNI/OpenNI/blob/master/README) and that on  [SensorKinect GitHub site](https://github.com/avin2/SensorKinect/blob/unstable/README).  
 


* The Core thing for this is Installing Driver for Kinect on Ubuntu. The two important packages you should install are:  
 The OpenNI UNSTABLE version  
 The SensorKinect package
* Here, I want to say that: after you have installed the OpenNI, and be ready to clone the git files for SensorKinect,DO NOT change directory!Otherwise you cannot generate the Redist directory for installation of SensorKinect.
* The Cloning process could be very slow, you can checkout the files from here:  
 http://yunpan.cn/cmTPmvr2D29Yj   keycode: 32e4
* There existed a Readme.txt file which is wriiten in Chinese. Don't worry, you can ignore it after you suffer this Installation for longer than 1 week cuz you will be the expert on this field at that time. ;-)
* When you succeded in using Kinect\_for\_Windows on Ubuntu, the using of Kinect\_for\_Xbox is just 'unplug and plugin'.
* It's really easier if you use Kinect\_for\_XBox with OpenKinect. unfortunately, OpenKinect does not support Kinect4Windows.
* If you're using Virtual Machine launching Ubuntu on it, I don't recommend you continue. I failed too many times in trying to do it on VMs. And finally I succeded in real machine.



## Read Carefully about This Blog:  [this blog](http://www.20papercups.net/programming/kinect-on-ubuntu-with-openni/). and follow it. Good luck!


my seccess of running ./Sample-NiSimpleViewer :


    ![](https://img-blog.csdn.net/20150827155546015?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


## If you still met some issues, my note of Installation can help you maybe.



================= Notes for debugging =============================



```
Optional: for ubuntu12.04_for_ros_hydro OS version, we need to firstly do some patch work:
  $ sudo apt-get update --fix-missing

------Install the Prerequisition------
The Prerequisition for your next Installation is the Tools you may need, Ubuntu will automatically install or upgrade them.
>>git 
>>build-essential 
>>python libusb-1.0-0-dev 
>>freeglut3-dev 
>>openjdk-7-jdk 
Run the Command:
  $ sudo apt-get install git build-essential python libusb-1.0-0-dev freeglut3-dev openjdk-7-jdk
  $ sudo apt-get install doxygen graphviz mono-complete
  $ cd ~/Downloads
  $ git clone https://github.com/OpenNI/OpenNI.git  
  $ cd OpenNI  
  $ git checkout Unstable-1.5.4.0  
  $ cd Platform/Linux/CreateRedist  
  $ chmod +x RedistMaker  
  $ ./RedistMaker  
---- Install the OpenNI UNSTABLE Version ----
  $ cd ../Redist/OpenNI-Bin-Dev-Linux-[xxx]    
   //(where [xxx] is your architecture and this particular OpenNI release)  
  $ sudo ./install.sh  

//  ---- Have a test: (Optional)-----
//  $ cd /home/exbot/Downloads/OpenNI/Platform/Linux/Bin/x86-Release
//  $ ./Sample-NiSimpleViewer 
//  we got:
//    ----------
//    One or more of the following nodes could not be enumerated:
//    >>
//    >>
//    >>
//    exbot@ubuntu:~/Downloads/OpenNI/Platform/Linux/Bin/x86-Release$
//    ----------
  Let's continue to Install the SensorKinect driver for OpenNI.

---- Install the SensorKinect Model ----  
Notice: Here You Should go to the path:
  /home/exbot/Downloads/OpenNI/Platform/Linux/Redist/OpenNI-Bin-Dev-Linux-[xxx]
  //the [xxx] is your OS version, mine is: x86-v1.5.4.0
Run these Command:

  $ git clone https://github.com/avin2/SensorKinect  
  $ cd SensorKinect/Platform/Linux/CreateRedist  
  $ chmod +x RedistMaker  
  $ sudo ./RedistMaker  

---- Install SensorKinect ----
  $ cd ../Redist/Sensor-Bin-Linux-[xxx]   
  //(where [xxx] is your architecture and this particular OpenNI release)  
  $ sudo chmod +x install.sh  
  $ sudo ./install.sh  

---- Have a test about your Installation work!! ----
Plugin your Kinect Sensor to the USB port. Test if linux detected your usb device:
  $ lsusb
Go to the OpenNI's path and run the SimpleSampleViewer:
  $ cd ~/Downloads/OpenNI/Platform/Linux/Bin/x86-Release/
  $ ./Sample-NiSimpleViewer

I finnally got the Depth Graph on my monitor!!!! I'm Really Excited after two weeks' working on this issue.

![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  
 


### The Issues I met:


**Open failed: Xiron OS failed to wait on event!**  
 


I met the error below when I was trying to test the kinect\_for\_Windows. :(  
 


$ ./Sample-NiSimpleViewer  
 Open failed: Xiron OS failed to wait on event!  
 


I'll try the installation process for OpenNI unstable, SensorKinect on a new clear Ubuntu\_12.04.5\_desktop\_i386. and solved.


  
 


  
 


## === English Ends ===


## 中文来了：在 [Ubuntu](https://so.csdn.net/so/search?q=Ubuntu&spm=1001.2101.3001.7020) 使用OpenNI驱动 Kincet 传感器


**在阅读中文之前，有些重要的点，我后来补充的，在前面英文里。所以欲练此功，必读英语。**  
 经过本人1周多对这个问题的不断调研，逐渐摸清一些经验，总结如下：


  1. 这个问题足以写一本10多页的教程。


  2. 虚拟机还是实体机、[VMWare](https://so.csdn.net/so/search?q=VMWare&spm=1001.2101.3001.7020) 还是 VirtualBox 虚拟机工具，都可能导致不同的结果。


  3. Kinect 这个传感器现在有至少3个版本（[difference between the two types of Kinect](http://choorucode.com/2013/07/23/difference-between-kinect-for-windows-and-kinect-for-xbox-360-on-linux/ "Difference between Kinect for Windows and Kinect for Xbox 360 on Linux")）。Kinect V2 在2014年发布了，此前是 Kinect\_for\_XBox & Kinect\_for\_Windows俩版本。


  4. 俩版本的Kinect在使用哪个Linux驱动 for Kinect working on Ubuntu是不太一样的，有的好用，有的不好用。


首先我们讨论 Using Kinect\_for\_Windows on Ubuntu 12.04, 然后讨论 Using Kinect\_for\_XBox on Ubuntu 12.04


  5. 了解一些关于Kinect, PrimeSense, OpenNI的背景知识，有助于理解和寻找有关技术资料。<https://en.wikipedia.org/wiki/OpenNI>  
 


  
 


写在前面：这个问题为什么带来这么大的难度和困扰，主要是因为OpenNI的创始团队PrimeSense被苹果收购了，OpenNI.org网站也在2014年4月被关闭。好在[GitHub](https://so.csdn.net/so/search?q=GitHub&spm=1001.2101.3001.7020)网站代码还健在。[OpenNI To Close](http://www.i-programmer.info/news/194-kinect/7004-openni-to-close-.html)    而**好好阅读OpenNI和avin2/SensorKincect的GitHub网站内的说明文档**，对于成功安装Ubuntu下的Kinect驱动程序是至关重要的。  
 


### 第一章： Using Kinect\_for\_Windows on Ubuntu 12.04


配置： PC台式机，原生安装 Ubuntu12.04\_x86.iso, 非双系统


参考：[Kinect on Ubuntu with OpenNI](http://www.20papercups.net/programming/kinect-on-ubuntu-with-openni/http://www.20papercups.net/programming/kinect-on-ubuntu-with-openni/http://www.20papercups.net/programming/kinect-on-ubuntu-with-openni/http://www.20papercups.net/programming/kinect-on-ubuntu-with-openni/http://www.20papercups.net/programming/kinect-on-ubuntu-with-openni/)  
 



================= Notes for debugging =============================



```
Optional: for ubuntu12.04_for_ros_hydro OS version, we need to firstly do some patch work:
  $ sudo apt-get update --fix-missing

------Install the Prerequisition------
The Prerequisition for your next Installation is the Tools you may need, Ubuntu will automatically install or upgrade them.
>>git 
>>build-essential 
>>python libusb-1.0-0-dev 
>>freeglut3-dev 
>>openjdk-7-jdk 
Run the Command:
  $ sudo apt-get install git build-essential python libusb-1.0-0-dev freeglut3-dev openjdk-7-jdk
  $ sudo apt-get install doxygen graphviz mono-complete
  $ cd ~/Downloads
  $ git clone https://github.com/OpenNI/OpenNI.git  
  $ cd OpenNI  
  $ git checkout Unstable-1.5.4.0  
  $ cd Platform/Linux/CreateRedist  
  $ chmod +x RedistMaker  
  $ ./RedistMaker  
---- Install the OpenNI UNSTABLE Version ----
  $ cd ../Redist/OpenNI-Bin-Dev-Linux-[xxx]    
   //(where [xxx] is your architecture and this particular OpenNI release)  
  $ sudo ./install.sh  

//  ---- Have a test: (Optional)-----
//  $ cd /home/exbot/Downloads/OpenNI/Platform/Linux/Bin/x86-Release
//  $ ./Sample-NiSimpleViewer 
//  we got:
//    ----------
//    One or more of the following nodes could not be enumerated:
//    >>
//    >>
//    >>
//    exbot@ubuntu:~/Downloads/OpenNI/Platform/Linux/Bin/x86-Release$
//    ----------
  Let's continue to Install the SensorKinect driver for OpenNI.

---- Install the SensorKinect Model ----  
Notice: Here You Should go to the path:
  /home/exbot/Downloads/OpenNI/Platform/Linux/Redist/OpenNI-Bin-Dev-Linux-[xxx]
  //the [xxx] is your OS version, mine is: x86-v1.5.4.0
Run these Command:

  $ git clone https://github.com/avin2/SensorKinect  
  $ cd SensorKinect/Platform/Linux/CreateRedist  
  $ chmod +x RedistMaker  
  $ sudo ./RedistMaker  

---- Install SensorKinect ----
  $ cd ../Redist/Sensor-Bin-Linux-[xxx]   
  //(where [xxx] is your architecture and this particular OpenNI release)  
  $ sudo chmod +x install.sh  
  $ sudo ./install.sh  

---- Have a test about your Installation work!! ----
Plugin your Kinect Sensor to the USB port. Test if linux detected your usb device:
  $ lsusb
Go to the OpenNI's path and run the SimpleSampleViewer:
  $ cd ~/Downloads/OpenNI/Platform/Linux/Bin/x86-Release/
  $ ./Sample-NiSimpleViewer

I finnally got the Depth Graph on my monitor!!!! I'm Really Excited after two weeks' working on this issue.

![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  
 


### The Issues I met:


**Open failed: Xiron OS failed to wait on event!**  
 


I met the error below when I was trying to test the kinect\_for\_Windows. :(  
 


$ ./Sample-NiSimpleViewer  
 Open failed: Xiron OS failed to wait on event!  
 


I'll try the installation process for OpenNI unstable, SensorKinect on a new clear Ubuntu\_12.04.5\_desktop\_i386. and solved.


  
 


  
 



### 第二章： Using Kinect\_for\_XBox on Ubuntu 12.04


配置： PC台式机，原生安装 Ubuntu12.04\_x86.iso, 非双系统。


当在Ubuntu 中玩起来了 Kinect\_for\_Windows, 我拔掉它，插上 Kinect\_for\_Xbox, 效果是一样的。


When you succeded in using Kinect\_for\_Windows on Ubuntu, the using of Kinect\_for\_Xbox is just 'unplug and plugin'.  
 


我建议使用OpenKinect.org提供的驱动。**OpenKinect** is an open community of people interested in making use of the amazing**Xbox Kinect** hardware with our PCs and other devices. We are working on free, open source libraries that will enable the Kinect to be used with Windows, Linux, and Mac.


如何安装Ubuntu驱动for Kinect\_for\_XBox及相关配置： http://openkinect.org/wiki/Getting\_Started


[Getting up and running with the Kinect in Ubuntu 12.04](http://blog.jozilla.net/2012/03/29/getting-up-and-running-with-the-kinect-in-ubuntu-12-04/)


  
 


### ============== The Original Blog on 20papercups.net ============


Kinect on Ubuntu with OpenNI  
 (Source:http://www.20papercups.net/programming/kinect-on-ubuntu-with-openni/)  
   
 UPDATE October 2015: Verified working in Ubuntu 14.04 LTS and 15.04!  
   
 I’ve spent all this morning trying to talk to the Microsoft Kinect using OpenNI. As it turns out, the process is not exceptionally difficult, it’s just there doesn’t seem to be any up to date documentation on getting it all working. So, this post should fill the void. I describe how to get access to the Kinect working using Ubuntu 12.04 LTS, OpenNI 1.5.4, and NITE 1.5.2.  
   
 Please note that since writing this tutorial, we now have OpenNI and NITE 2.0, and PrimeSense have been bought by Apple. This tutorial does not work with versions 2 (though 1.5 works just fine), and there is talk of Apple stopping public access to NITE.  
   
 To talk to the Kinect, there are two basic parts: OpenNI itself, and a Sensor module that is actually responsible for communicating with the hardware. Then, if you need it, there is NITE, which is another module for OpenNI that does skeletal tracking, gestures, and stuff. Depending on how you plan on using the data from the Kinect, you may not need NITE at all.  
   
 Step 1: Prerequisites  
   
 We need to install a bunch of packages for all this to work. Thankfully, the readme file included with OpenNI lists all these. However, to make life easier, this is (as of writing) what you need to install, in addition to all the development packages you (hopefully) already have.  
   
 $ sudo apt-get install git build-essential python libusb-1.0-0-dev freeglut3-dev openjdk-7-jdk  
   
 There are also some optional packages that you can install, depending on whether you want documentation, Mono bindings, etc. Note that on earlier versions the install failed if you didn’t have doxygen installed, even though it is listed as optional.  
   
 $ sudo apt-get install doxygen graphviz mono-complete  
   
   
   
 ==============================  
 Step 2: OpenNI 1.5.4  
   
 OpenNI is a framework for working with what they are calling natural interaction devices.Anyway, this is how it is installed:  
   
 Check out from Git  
   
 OpenNI is hosted on Github, so checking it out is simple:  
   
 >> git clone https://github.com/OpenNI/OpenNI.git  
   
 The first thing we will do is checkout the Unstable 1.5.4 tag. If you don’t do this, then the SensorKinect library won’t compile in Step 3. From there, change into the Platform/Linux-x86/CreateRedist directory, and run the RedistMaker script. Note that even though the directory is named x86, this same directory builds 64 bit versions just fine. So, don’t fret if you’re on 64bit Linux.  
   
 >> cd OpenNI  
 >> git checkout Unstable-1.5.4.0  
 >> cd Platform/Linux/CreateRedist  
 >> chmod +x RedistMaker  
 >> ./RedistMaker  
   
 The RedistMaker script will compile everything for you. You then need to change into the Redist directory and run the install script to install the software on your system.  
   
 >> cd ../Redist/OpenNI-Bin-Dev-Linux-[xxx]  (where [xxx] is your architecture and this particular OpenNI release)  
 >> sudo ./install.sh  
   
 ============================  
 Step 3: Kinect Sensor Module  
   
 OpenNI doesn’t actually provide anything for talking to the hardware, it is more just a framework for working with different sensors and devices. You need to install a Sensor module for actually doing the hardware interfacing. Think of an OpenNI sensor module as a device driver for the hardware. You’ll also note on the OpenNI website that they have a Sensor module that you can download. Don’t do this though, because that sensor module doesn’t talk to the Kinect. I love how well documented all this is, don’t you?  
   
 The sensor module you want is also on GitHub, but from a different user. So, we can check out the code. We also need to get the kinect branch, not master.  
   
 >>git clone https://github.com/avin2/SensorKinect  
 >>cd SensorKinect  
   
 The install process for the sensor is pretty much the same as for OpenNI itself:  
   
 >>cd Platform/Linux/CreateRedist  
 >>chmod +x RedistMaker  
 >>./RedistMaker  
 >>cd ../Redist/Sensor-Bin-Linux-[xxx] (where [xxx] is your architecture and this particular OpenNI release)  
 >>chmod +x install.sh  
 >>sudo ./install.sh  
   
 On Ubuntu, regular users are only given read permission to unknown USB devices. The install script puts in some udev rules to fix this, but if you find that none of the samples work unless you run them as root, try unplugging and plugging the Kinect back in again, to make the new rules apply.  
   
 ===============================  
 Step 4: Test the OpenNI Samples  
   
 At this point, you have enough installed to get data from the Kinect. The easiest way to verify this is to run one of the OpenNI samples.  
   
 >>cd OpenNI/Platform/Linux-x86/Bin/Release  
 >>./Sample-NiSimpleViewer  
   
 You should see a yellow-black depth image. At this point, you’re left with (optionally) installing the higher level NITE module.  
   
   
 ===================================  
 Step 5: Install NITE 1.5 (optional)  
   
 Firstly, you need to obtain NITE 1.5.2. Go to the following link and download NITE 1.5.2 for your platform..  
   
 http://www.openni.org/openni-sdk/openni-sdk-history-2/  
   
 Extract the archive, and run the installer:  
   
 >>sudo ./install.sh  
   
 At some point, you may be asked for a license key. A working license key can be found just about anywhere on the Internet. I don’t think PrimeSense care, or maybe this is a non-commercial license or something. But whatever, just copy that license into the console, including the equals sign at the end, and NITE will install just fine.  
 Conclusion  
   
 After following these steps, you will be able to write programs that use the Microsoft Kinect through OpenNI and NITE middleware. I hope this helps someone, because I spent a lot of time screwing around this morning trying to get it all to work. Like I said, the process is pretty straight forward, it just hasn’t been written down in one place (or I suck at google).  
 


  

  
 





---
layout: post
title:  "在ROS使用 Kinect 完成3D图像处理（1）-- 测试OpenNI_camera"
date: 2015-09-15 13:57:01 +0800
tags: 
slug: p20150915135701
---

# 在ROS使用 Kinect 完成3D图像处理（1）-- 测试OpenNI_camera





## 在ROS使用 [Kinect](https://so.csdn.net/so/search?q=Kinect&spm=1001.2101.3001.7020) 完成3D图像处理（1）-- 测试OpenNI\_camera


在完成了  [在 Ubuntu 用 OpenNI支持 使用 Kincet 传感器](http://blog.csdn.net/sonictl/article/details/47974485) 之后，我们可以尝试使用ROS提供的包来测试ROS+Kinect有关的功能  
 


  

ROS 有两个包，支持kinect在ROS上的运行： openni\_camera & openni\_launch, 前者应该是driver, 后者应该是运行程序。  
 


ROS [Package](https://so.csdn.net/so/search?q=Package&spm=1001.2101.3001.7020): http://wiki.ros.org/openni\_camera  
                         http://wiki.ros.org/openni\_launch   (OPTIONAL)  
 



#### ROS openni-camera installation on ubuntu



在完成了  [在 Ubuntu 用 OpenNI支持 使用 Kincet 传感器](http://blog.csdn.net/sonictl/article/details/47974485) 之后，我们可以尝试使用ROS提供的包来测试ROS+Kinect有关的功能.


To install the ROS OpenNI drivers for the Microsoft Kinect or Asus Xtion, use the command:  
 



```
    $ sudo apt-get install ros-hydro-openni-camera

```


#### Testing your Kinect or Xtion Camera

 Once you have the OpenNI driver installed, make sure you can see the video stream from the camera by using the ROS image\_view package. For the Kinect or Xtion, first plug the camera in to any available USB port (and for the Kinect, make sure it has power through its 12V adapter or other means), then prepare to run the following launch file: 
  


    Here you can select which launch file you prefer to install:


    1. offered by ROS\_by\_Example book:   
 


           Here you should refer the book at Chapter5. INSTALLING THE ROS-BY-EXAMPLE CODE. And install the rbx1 packages.  
            Then, launch the file and publish the data as **/camera/rgb/image\_color** &**/camera/depth/disparity** topics:  
 



```
    $ roslaunch rbx1_vision openni_node.launch 
```
   
  


    2. offered by ROS community:   
 



```
    $ sudo apt-get install ros-hydro-openni-launch

```


          Then, launch the file and publish the data as **/camera/rgb/image\_color** &**/camera/depth/disparity** topics:  
 



```
    $ roslaunch openni_launch openni.launch
```

    I recommend you install both the rbx1 and the openni\_launch...  
 

    Now launch either of the two launch file via one of the commands above. 
  
    If the connection to the camera is successful, you should see a series of diagnostic messages that look something like this: 
  

   [ INFO] [1384877526.285283926]: Number devices connected: 1  
    [ INFO] [1384877526.285412166]: 1. device on bus 002:77 is a SensorV2  
    (2ae) from PrimeSense (45e) with serial id 'A00365A20145047A'  
    [ INFO] [1384877526.286543142]: Searching for device with index = 1  
    [ INFO] [1384877526.341836602]: Opened 'SensorV2' on bus 2:77 with  
    serial number 'A00365A20145047A'  
    process[camera\_base\_link2-20]: started with pid [17928]  
    process[camera\_base\_link3-21]: started with pid [17946]  
    [ INFO] [1384877527.436366816]: rgb\_frame\_id =  
    '/camera\_rgb\_optical\_frame'  
    [ INFO] [1384877527.436487549]: depth\_frame\_id =  
    '/camera\_depth\_optical\_frame'  
 NOTE: Don't worry if you see an error message that begins "Skipping XML Document..." or a few warning warning messages about the camera calibration file not being found. These messages can be ignored.


#### View the RGB/Depth video stream


Next, use the ROS image\_view utility to view the RGB video stream. **openni\_camera** have set up our camera launch files so that the color video stream is published on the ROS topic**/camera/rgb/image\_color** . To view the video, we therefore run:  
 



```
   $ rosrun image_view image_view image:=/camera/rgb/image_color
```

A small camera display window should pop up and after a brief delay, you should see the live video stream from your camera. Move something in front of the camera to verify that the image updates appropriately. Once you are satisfied that you have a live video, close the image\_view window or type Ctrl-C in the terminal from which you launched it.  
 


You can also test the depth image from your camera using the ROS disparity\_view node:  
 



```
   $ rosrun image_view disparity_view image:=/camera/depth/disparity
```
 In this case, the resulting colors in the image represent depth, with red/yellow indicating points closer to the camera, blue/violet representing points further away and green for points with intermediate depth. Note that if you move an object inside the camera's minimum depth range (about 50cm), the object will turn grey indicating that the depth value is no longer valid at this range. 
  

#### What's next?


The next you should do is using some utilities offered by ROS, OpenCV, or yourself to do the image processing, image recognition, object detection, facetracking and so on.What the processing nodes should do is to firstly listen to the topics**/camera/rgb/image\_color** &**/camera/depth/disparity** and get the data to do the processing work.  
   
 


  
   
 





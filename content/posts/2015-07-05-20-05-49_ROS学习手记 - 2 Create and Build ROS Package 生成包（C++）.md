---
layout: post
title:  "ROS学习手记 - 2 Create and Build ROS Package 生成包（C++）"
date: 2015-07-05 20:05:49 +0800
tags: 
slug: p20150705200549
---

# ROS学习手记 - 2 Create and Build ROS Package 生成包（C++）





For ROS experienced user, You can just skip to the Constuction about ROS [Package](https://so.csdn.net/so/search?q=Package&spm=1001.2101.3001.7020) usage:


For Quick Reference(C++): <http://blog.csdn.net/sonictl/article/details/46764855#t5>


For Quick Reference(Python): <http://blog.csdn.net/sonictl/article/details/46764855#t15>  
 


=== 在上一篇的基础上，Create并Customize了Package之后，要Build Package ===  
 


来源：http://wiki.ros.org/ROS/Tutorials/BuildingPackages  
 


#### 1. 确认source有没有完成 (版本： hydro)：




```
   $ source /opt/ros/hydro/setup.bash

```

  


#### 2. Using catkin\_make


      使用方法：



```
   # In a catkin workspace
   $ catkin_make [make_targets] [-DCMAKE_VARIABLES=...]
```
      这里catkin\_make使用了CMake 的workflow来操作，科普一下这个workflow: 

     ![](https://img-blog.csdn.net/20150705193504546?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


#### 3. 如何写ROS的Publisher and Subscriber(C++)


       因为要涉及到对CMakeLists.txt的写法，需要先了解这个文章：http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29  
        具体关于CMakeLists.txt文件，请看http://wiki.ros.org/catkin/CMakeLists.txt


#### 4. 关于workspace下的build devel src文件夹


        The build folder is the default location of the[build space](http://wiki.ros.org/catkin/workspaces#Build_Space) and is wherecmake andmake are called to configure and build your packages.  
         The devel folder is the default location of the[devel space](http://wiki.ros.org/catkin/workspaces#Development_.28Devel.29_Space), which is where your executables and libraries go before you install your packages.  
   
 


#### 5. Build the package


       catkin\_make命令执行情况如下：  
 



```
exbot@ubuntu:~/catkin_ws$ ls
build  devel  src
exbot@ubuntu:~/catkin_ws$ catkin_make
Base path: /home/exbot/catkin_ws
Source space: /home/exbot/catkin_ws/src
Build space: /home/exbot/catkin_ws/build
Devel space: /home/exbot/catkin_ws/devel
Install space: /home/exbot/catkin_ws/install
####
#### Running command: "make cmake_check_build_system" in "/home/exbot/catkin_ws/build"
####
-- Using CATKIN_DEVEL_PREFIX: /home/exbot/catkin_ws/devel
-- Using CMAKE_PREFIX_PATH: /opt/ros/hydro
-- This workspace overlays: /opt/ros/hydro
-- Using PYTHON_EXECUTABLE: /usr/bin/python
-- Python version: 2.7
-- Using Debian Python package layout
-- Using CATKIN_ENABLE_TESTING: ON
-- Call enable_testing()
-- Using CATKIN_TEST_RESULTS_DIR: /home/exbot/catkin_ws/build/test_results
-- Found gtest sources under '/usr/src/gtest': gtests will be built
-- catkin 0.5.86
-- BUILD_SHARED_LIBS is on
-- ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-- ~~  traversing 7 packages in topological order:
-- ~~  - beginner_tutorials
-- ~~  - exbotxi_bringup
-- ~~  - exbotxi_description
-- ~~  - exbotxi_example
-- ~~  - exbotxi_nav
-- ~~  - exbotxi_rviz
-- ~~  - exbotxi_teleop
-- ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
-- +++ processing catkin package: 'beginner_tutorials'
-- ==> add_subdirectory(beginner_tutorials)
-- +++ processing catkin package: 'exbotxi_bringup'
-- ==> add_subdirectory(exbot_xi/exbotxi_bringup)
-- +++ processing catkin package: 'exbotxi_description'
-- ==> add_subdirectory(exbot_xi/exbotxi_description)
-- +++ processing catkin package: 'exbotxi_example'
-- ==> add_subdirectory(exbot_xi/exbotxi_example)
-- +++ processing catkin package: 'exbotxi_nav'
-- ==> add_subdirectory(exbot_xi/exbotxi_nav)
-- +++ processing catkin package: 'exbotxi_rviz'
-- ==> add_subdirectory(exbot_xi/exbotxi_rviz)
-- +++ processing catkin package: 'exbotxi_teleop'
-- ==> add_subdirectory(exbot_xi/exbotxi_teleop)
-- Configuring done
-- Generating done
-- Build files have been written to: /home/exbot/catkin_ws/build
####
#### Running command: "make -j1 -l1" in "/home/exbot/catkin_ws/build"
####
[100%] Built target move
exbot@ubuntu:~/catkin_ws$ 

```
        
  
        虽然跟教程上有点不同，但没遇到错误，就这样吧。。 
  


  
 


### ======  【总结 for Quick Reference (C++)】  ======


#### Constuction about ROS Package:


1. Create ROS Workspace(catkin)
2. Create ROS Package (catkin)
3. Build ROS Package (catkin)
4. Delete ROS Package (catkin)


##### == 1. Create ROS Workspace (catkin)==


1. create a catkin workspace  
 



```
   $ mkdir -p ~/catkin_ws/src
   $ cd ~/catkin_ws/src
   $ catkin_init_workspace
```

2. Make  
 Even though the workspace is empty (there are no packages in the 'src' folder, just a single CMakeLists.txt link) you can still "build" the workspace:  
 



```
     $ cd ~/catkin_ws/
     $ catkin_make
```

  
 3. Source  
 



```
    $ source devel/setup.bash
```

  
 4. Check:  
 



```
     $ echo $ROS_PACKAGE_PATH
```

/home/youruser/catkin\_ws/src:/opt/ros/indigo/share:/opt/ros/indigo/stacks  
   
 


##### === 2. Create an Empty ROS Package (catkin) ===


1. 确认你的catkin Workspace：~/catkin\_ws/  
 



```
2.     $ cd ~/catkin_ws/src
```

3. 创建package  
 



```
     $ catkin_create_pkg <package_name> [depend1] [depend2] [depend3] ... [dependn]
```

     # for example :



```
   $ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
```

  
 4. 编译catkin Workspace: 
  


```
     $ cd ~/catkin_ws
     $ catkin_make
```
 5. Source the generated setup file: 
  


```
     $ . ~/catkin_ws/devel/setup.bash
```

  


##### === 3. Customize ROS Package (catkin) ===


* Add / Edit the source code file in your <package>/src/ path
* Customize the package.xml ref this  [page](http://wiki.ros.org/ROS/Tutorials/CreatingPackage#ROS.2BAC8-Tutorials.2BAC8-catkin.2BAC8-CreatingPackage.Customizing_the_package.xml)
* Customize the  [CMakeLists.txt](http://wiki.ros.org/ROS/Tutorials/CreatingPackage#ROS.2BAC8-Tutorials.2BAC8-catkin.2BAC8-CreatingPackage.Customizing_the_CMakeLists.txt)


1. Edit the code in ~/catkin\_ws/src/<package\_name>/src/


     Add source code file: SouceCode.cpp or SourceCode.py  etc.


2. Modify the CMakeFile.txt , (refer the details about CMakeLists.txt :  [link](http://wiki.ros.org/catkin/CMakeLists.txt) )  
 


     Modify the CMakeList.txt file:  
 



```
    $ cd ~/catkin_ws/src/<package_name>
    $ gedit CMakeList.txt

```

     add/modify these lines as below:



```
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_executable(<package_name> src/<soucefile_name>.cpp)

add_dependencies(<package_name> ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(<package_name>
   ${catkin_LIBRARIES}
 )

![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

##### === 4. Build the ROS Package (catkin) ===


ref page:  [link](http://wiki.ros.org/ROS/Tutorials/BuildingPackages)  
 


1. Source your environment setup file
2. catkin\_make [make\_targets] [-DCMAKE\_VARIABLES=...]


1. Source env setup file: （Optional）  
 




```
   $ source /opt/ros/%YOUR_ROS_DISTRO%/setup.bash
```
 2. Catkin\_make 
  


```
  $ cd ~/catkin_ws
  $ catkin_make -DCMAKE_BUILD_TYPE=Release 
```


For more advanced uses of  [catkin\_make](http://wiki.ros.org/catkin/commands/catkin_make) see the documentation:  [catkin/commands/catkin\_make](http://wiki.ros.org/catkin/commands/catkin_make)  
 


To add the workspace to your ROS environment you need to source the generated setup file:  
 



```
  $ . ~/catkin_ws/devel/setup.bash

```

3. Rebuilding a Single catkin Package  
 


If you update a single package in your catkin workspace and want to re-build just that package, use the following variation of catkin\_make:  
 



```
   $ cd ~/catkin_ws
   $ catkin_make --pkg <package_name>
```



##### === 5. Use ROS Package (catkin) ===



Here we also offere a simple launch file that interfaces to two microcontrollers using two serial connections, put the code into  
   "~/catkin\_ws/src/<package\_name>/launch/<launchfile\_name>.launch" file:  
 



```
1. <launch>
2. <node pkg="r2SerialDriver" type="r2SerialDriver" name="r2Serial0" args="0 /dev/ttyUSB0 9600" output="screen" >
3. <remap from="ucCommand" to="uc0Command" />
4. <remap from="ucResponse" to="uc0Response" />
5. </node>
6. 
7. <node pkg="r2SerialDriver" type="r2SerialDriver" name="r2Serial1" args="1 /dev/ttyUSB1 9600" output="screen" >
8. <remap from="ucCommand" to="uc1Command" />
9. <remap from="ucResponse" to="uc1Response" />
10. </node>
11. 
12. </launch>

```

Usage:



```
$ rosrun <package_name> <node_name> arg1 arg2 arg3 ...

```

or



```
$ roslaunch <package_name> <launchfile_name>.launch

```

  

##### === 6. Delete ROS package ===


##### === 6. Doing a "make clean" with catkin ===

 When using the older rosmake build system, you could use the command: 
  


```
      $  rosmake --target=clean
```
 in a top level stack or package directory to remove all build objects. Unfortunately, this feature does not exist when using catkin. The only way to start with a clean slate, is to remove all build objects from all your catkin packages using the following commands: 
  
     
  
     CAUTION! 
**Do not** include the 
 ***src*** directory in the rm command below or you will lose all your personal catkin source files! 
  


```
      $ cd ~/catkin_ws
      $ \rm -rf devel build install
```
 You would then remake any packages as usual: 
  


```
      $ cd ~/catkin_ws
      $ catkin_make
      $ source devel/setup.bash
```

##### === 7. Finally: 总结Package的基本概念 ===


1. 相对ROS的角色和地位。
2. Package文件夹至少要包含的文件：package.xml & CMakeLists.txt &
3. 拒绝嵌套
4. Packages 在catkin workspace中的文件分布结构
5. Package Contains two necessary elements: 
```
  CMakeLists.txt
  package.xml
```
6. 


  
   
   
 


### ======  【总结 for Quick Reference (Python)】  ======


refer: <http://wiki.ros.org/rospy_tutorials>  
 



#### Constuction about ROS Package:


1. Create ROS Workspace(catkin)
2. Create ROS Package (catkin)
3. Build ROS Package (catkin)
4. Delete ROS Package (catkin)


##### == 1. Create ROS Workspace (catkin)==



1. create a catkin workspace  
 



```
   $ mkdir -p ~/catkin_ws/src
   $ cd ~/catkin_ws/src
   $ catkin_init_workspace
```

2. Make  
 Even though the workspace is empty (there are no packages in the 'src' folder, just a single CMakeLists.txt link) you can still "build" the workspace:  
   
 



```
     $ cd ~/catkin_ws/
     $ catkin_make
```

  
 3. Source  
 



```
     $ source devel/setup.bash
```

4. Check:  
 



```
     $ echo $ROS_PACKAGE_PATH
```

/home/youruser/catkin\_ws/src:/opt/ros/indigo/share:/opt/ros/indigo/stacks  
   
 


##### === 2. Create an Empty ROS Package (catkin) ===


1. 确认你的catkin Workspace：~/catkin\_ws/  
 



```
     $ cd ~/catkin_ws/src
```

2. 创建package  
 



```
     $ catkin_create_pkg <package_name> [depend1] [depend2] [depend3] ... [dependn]
```

     # for example :



```
   $ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
```

  
 4. 编译catkin Workspace: 
  


```
     $ cd ~/catkin_ws
     $ catkin_make
```
 5. Source the generated setup file: 
  


```
     $ . ~/catkin_ws/devel/setup.bash
```

  

##### === 3. Customize ROS Package (catkin) ===


* Add / Edit the source code file in your  <package>/scripts/ path
* Customize the package.xml ref this  [page](http://wiki.ros.org/ROS/Tutorials/CreatingPackage#ROS.2BAC8-Tutorials.2BAC8-catkin.2BAC8-CreatingPackage.Customizing_the_package.xml)
* Customize the  [CMakeLists.txt](http://wiki.ros.org/ROS/Tutorials/CreatingPackage#ROS.2BAC8-Tutorials.2BAC8-catkin.2BAC8-CreatingPackage.Customizing_the_CMakeLists.txt)


1. Edit the code in ~/catkin\_ws/src/<package\_name>/src/


     Add source code file: SouceCode.cpp or SourceCode.py  etc.


2. Modify the CMakeFile.txt , (refer the details about CMakeLists.txt for Python:[Writing a ROS Python Makefile](http://wiki.ros.org/rospy_tutorials/Tutorials/Makefile) )  
 


     Modify the CMakeList.txt file:  
 



```
$ cd ~/catkin_ws/src/<package_name>
$ gedit CMakeList.txt

```

     add/modify these lines as below:



```
 //#（You may not need to modify the CMakeList.txt file for your python proj. ）


```

##### === 4. Build the ROS Package (catkin) ===


ref page:  [link](http://wiki.ros.org/ROS/Tutorials/BuildingPackages)  
 


1. Source your environment setup file
2. catkin\_make [make\_targets] [-DCMAKE\_VARIABLES=...]


1. Source env setup file: （Optional）  
 



```
   $ source /opt/ros/%YOUR_ROS_DISTRO%/setup.bash
```
 2. Catkin\_make 
  


```
  $ cd ~/catkin_ws
  $ catkin_make -DCMAKE_BUILD_TYPE=Release 
```

For more advanced uses of  [catkin\_make](http://wiki.ros.org/catkin/commands/catkin_make) see the documentation:  [catkin/commands/catkin\_make](http://wiki.ros.org/catkin/commands/catkin_make)  
 


To add the workspace to your ROS environment you need to source the generated setup file:  
 



```
  $ . ~/catkin_ws/devel/setup.bash

```

3. Rebuilding a Single catkin Package  
 


If you update a single package in your catkin workspace and want to re-build just that package, use the following variation of catkin\_make:  
 



```
   $ cd ~/catkin_ws
   $ catkin_make --pkg <package_name>
```

##### === 5. Use ROS Package (catkin) ===


Here we also offere a simple launch file that interfaces to two microcontrollers using two serial connections, put the code into  
   "~/catkin\_ws/src/<package\_name>/launch/<launchfile\_name>.launch" file:  
 



```
1. <launch>
2. <node pkg="r2SerialDriver" type="r2SerialDriver" name="r2Serial0" args="0 /dev/ttyUSB0 9600" output="screen" >
3. <remap from="ucCommand" to="uc0Command" />
4. <remap from="ucResponse" to="uc0Response" />
5. </node>
6. 
7. <node pkg="r2SerialDriver" type="r2SerialDriver" name="r2Serial1" args="1 /dev/ttyUSB1 9600" output="screen" >
8. <remap from="ucCommand" to="uc1Command" />
9. <remap from="ucResponse" to="uc1Response" />
10. </node>
11. 
12. </launch>

```

Usage:



```
$ rosrun <package_name> <node_name> arg1 arg2 arg3 ...

```

or



```
$ roslaunch <package_name> <launchfile_name>.launch

```

  

##### === 6. Delete ROS package ===


##### === 6. Doing a "make clean" with catkin ===

 When using the older rosmake build system, you could use the command: 
  


```
      $  rosmake --target=clean
```
 in a top level stack or package directory to remove all build objects. Unfortunately, this feature does not exist when using catkin. The only way to start with a clean slate, is to remove all build objects from all your catkin packages using the following commands: 
  
     
  
     CAUTION! 
**Do not** include the 
 ***src*** directory in the rm command below or you will lose all your personal catkin source files! 
  


```
      $ cd ~/catkin_ws
      $ \rm -rf devel build install
```
 You would then remake any packages as usual: 
  


```
      $ cd ~/catkin_ws
      $ catkin_make
      $ source devel/setup.bash
```

  

  
 


  
 





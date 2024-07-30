---
layout: post
title:  "ROS学习手记 - 1了解并安装ROS+创建ROS_Package"
date: 2015-07-03 17:55:36 +0800
tags: 
slug: p20150703175536
---

# ROS学习手记 - 1了解并安装ROS+创建ROS_Package





  
 ROS学习手记：


#### 开始之前：

      先看了 
[《认识ROS操作系统(2015版)》](http://wenku.baidu.com/view/9e78c092a300a6c30c229f9a)，了解了ROS的基本概念。 
  
      这里补充一点ROS的发展历程，以便了解catkin的由来，及willow garage公司与ROS的关系： 
  
     ROS was originally developed in 2007 under the name switchyard by the Stanford Artificial Intelligence Laboratory in support of the Stanford AI Robot STAIR (STanford AI Robot) [6][7] project.From 2008 until 2013, development was performed primarily at Willow Garage, a robotics research institute/incubator. During that time, researchers at more than twenty institutions collaborated with Willow Garage engineers in a federated development model.[8][9] 
  
     In February 2013, ROS stewardship transitioned to the Open Source Robotics Foundation.[10] In August 2013, a blog posting[11] announced that Willow Garage would be absorbed by another company started by its founder, Suitable Technologies. The support responsibilities for the PR2 created by Willow Garage were also subsequently taken over by Clearpath Robotics.[12] （https://en.wikipedia.org/wiki/Robot\_Operating\_System） 
  

  
 


### 提纲：一台[Ubuntu](https://so.csdn.net/so/search?q=Ubuntu&spm=1001.2101.3001.7020)的PC主要要做以下两方面的工作



#### \* 安装 ros-hydro-desktop-full


               ref: <http://wiki.ros.org/hydro/Installation/Ubuntu>  
 


#### \* 配置workspace (catkin\_ws/ rosbuild\_ws/ 理解overlay)


* 配置 catkin\_ws (所有catkin类型包放在 ~/catkin\_ws/src/ 目录下编译或者运行)
* \* 配置 rosbuild\_ws (所有rosbuild类型包放在 ~/rosbuild\_ws/sandbox/ 目录下编译或者运行)
* \* 配置 overlay: rosbuild\_ws -> catkin\_ws -> hydro


                ref: <http://wiki.ros.org/catkin/Tutorials>  
 


  
 


下面我就具体展开。读者可以直接看上面俩链接步步操作还好点。  
 


#### 1. ROS 的安装，

       ROS官方Tutorial里的 
[Installing and Configuring Your ROS Environment](http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment)比较繁琐。我们采用虚拟机搭载Ubuntu\_for\_ROS的方式。 
  

  

      具备了虚拟机使用经验的同学可以搜“ 
Ubuntu for ROS”进行下载 .iso 文件在虚拟机里安装。笔者使用的是VirtualBox作为虚拟机工具。这里就不多说了。 
  

      需要注意的是，ROS有很多版本，对应Ubuntu不同的版本，别搞错了。 
我用的 Ubuntu12.04 + ROS\_Hydro（ubuntu12.04-i386-ros-exbot-h2-140520.iso）
  


  

      运行如下命令可看ROS的Package Path是否配置完毕。 
  

      >> echo $ROS\_PACKAGE\_PATH 
  

  

  

#### 2. 熟悉一下ROS的文件系统命令行

        
roscd, rosls, rospack 等命令的熟悉： 
  

#### 3. ROS Package


开始之前，ROS有这几个概念需要了解： 
  

  >>catkin - catkin is the official build system of ROS（相当于GNC Make，CMake这样的编译链接系统）. The name catkin comes from the tail-shaped flower cluster found on willow trees -- a reference to Willow Garage where catkin was created.（http://wiki.ros.org/catkin/conceptual\_overview） 
  

  >>Packages - ROS的基本组织单位，为了很好的定制和重用，包含库、数据集、节点、配置文件、第三方软件或任何有用的模型。Packages are the most atomic unit of build and the unit of release. 
  

  >>Metapackages -  
  

  >>Manifest 
  

  >>msg 
  

  preference: http://wiki.ros.org/Packages 
  

  >>srv 
  

##### -- Create an empty catkin workspace

    参考： http://wiki.ros.org/catkin/Tutorials/create\_a\_workspace 
  

  
    我装的ubuntu for ROS应该是已经source了/opt/ros/hydro/setup.bash，否则要运行： 
  
      $ source /opt/ros/hydro/setup.bash 
  

   开始来create a catkin workspace（我装的ubuntu for ROS应该是已经建立了workspace）:  
      $ mkdir -p ~/catkin\_ws/src  
      $ cd ~/catkin\_ws/src  
      $ catkin\_init\_workspace  
    但是我的这个路径下已经有了文件了：  
        exbot@ubuntu:~/catkin\_ws/src$ ls  
        CMakeLists.txt  exbot\_xi  
        cd ~/catkin\_ws/src/exbot\_xi && ls  
        exbotxi\_bringup      exbotxi\_example  exbotxi\_rviz    README.md  
        exbotxi\_description  exbotxi\_nav      exbotxi\_teleop
  

  
    看样子已经有很多包了，所以我就没搞 catkin\_init\_workspace 
  

#### 4. 创建Package


   跟着这个教程一步步来：http://wiki.ros.org/ROS/Tutorials/CreatingPackage  
    一个catkin Package包含的东西：1. package.xml file; 2.CMakeLists.txt; 3.其他文件.    
    Package不共用一个文件夹，nest(嵌套)也不行。  
   
   
    现在我们来Create一个Package：  
     $  cd ~/catkin\_ws/src  
     $  catkin\_create\_pkg beginner\_tutorials std\_msgs rospy roscpp  
        Created file beginner\_tutorials/CMakeLists.txt  
        Created file beginner\_tutorials/package.xml  
        Created folder beginner\_tutorials/include/beginner\_tutorials  
        Created folder beginner\_tutorials/src  
        Successfully created files in /home/exbot/catkin\_ws/src/beginner\_tutorials. Please adjust the values in package.xml.  
    Catkin\_create\_pkg requires that you give it a package\_name and optionally a list of dependencies on which that package depends.  
    use the catkin\_create\_pkg script to create a new package called 'beginner\_tutorials' which depends on std\_msgs, roscpp, and rospy.  
     
 

 == Building a catkin workspace and sourcing the setup file 
  

  
    Now you need to build the packages in the catkin workspace: 
  
      $ cd ~/catkin\_ws 
  
      $ catkin\_make     #尽管devel里已经有东西了，新建了一个catkin package就要catkin\_make一次 
  
    After the workspace has been built it has created a similar structure in the devel subfolder as you usually find under /opt/ros/$ROSDISTRO\_NAME. 这里ROSDISTRO\_NAME = hydro 
  
     
  
    -- ubuntu for ROS已有的文件结构 -- 
  
    ~/catkin\_ws 
  
       /build 
  
           
  
       /devel 
  
          env.sh  etc  lib  setup.bash  setup.sh  \_setup\_util.py  setup.zsh  share 
  
       /src 
  
          /CMakeLists.txt 
  
          /exbot\_xi 
  
             /exbotxi\_bringup 
  
             /exbotxi\_example 
  
             /exbotxi\_rviz 
  
             /exbotxi\_description 
  
             /exbotxi\_nav 
  
             /exbotxi\_teleop 
  
             README.md 
  
      /beginner\_tutorials   #这个就是这回新增的 
  
             /include 
  
             /src 
  
             CMakeLists.txt 
  
             package.xml 
  

     
 


##### -- add the workspace to your ROS environment

    To add the workspace to your ROS environment you need to source the generated setup file: 
  
       $ . ~/catkin\_ws/devel/setup.bash 
  
    这样，我们就可以通过roscd命令找到这个package: roscd beginner\_tutorials 
  

  
    至此，我们完成了Create, Build, Add to ROS Env三个步骤。 
  

  
 


#### == package dependencies

    用一个工具来看dependencies: rospack depends1 package\_name 
  
    结果运行以下命令时候出现问题： 
  
    $ rospack depends1 beginner\_tutorials 
  
    [rospack] Error: the rosdep view is empty: call 'sudo rosdep init' and 'rosdep update' 
  
    当运行sudo rosdep init时，提示要移除一些东西，再init再update. 
  
    于是，按照csdn有个博客的提示，需要如下依次执行： 
  
    $ sudo rm -rf $HOME/.ros/rosdep 
  
    $ sudo rm -rf /etc/ros/rosdep 
  
    $ sudo rm /etc/ros/rosdep/sources.list.d/20-default.list   
  
    $ sudo apt-get install python-rosdep 
  
    $ sudo rosdep init 
  
    $ rosdep update 
  

  
    再rospack depends1 package\_name： 
  
    $ rospack depends1 beginner\_tutorials 
  
      roscpp 
  
      rospy 
  
      std\_msgs 
  
    如果是rospack depends beginner\_tutorials即可显示所有包含nested的depends 
  
     
  

  

#### == Customizing Your Package 定制你的Package

    要一行行解释catkin\_create\_pkg命令建立的所有文件。 
  
    我们先来关注beginner\_tutorials/package.xml文件 
  
    开始之前我们关注一下package.xml文件的基础知识 
  
    -- package.xml 文件基础 -- 
  
    The package manifest is an XML file called package.xml that must be included with any catkin-compliant package's root folder. （http://wiki.ros.org/catkin/package.xml） 
  
    Other than a required <buildtool\_depends> dependency on catkin, metapackages can only have “run dependencies” on packages of which they group. 
  
    和要求的<buildtool\_depends> dependency 在catkin不同,metapackages 只能have run dependencies，这些是建立在它们group的package。太难翻了! 
  

   关于dependencies，这里有一段解释：  
     




| Programs often use some of the same files as each other. Rather than putting these files into each package, a separate package can be installed to provide them for all of the programs that need them. So, to install a program which needs one of these files, the package containing those files must also be installed. When a package depends on another in this way, it is known as a package dependency. By specifying dependencies, packages can be made smaller and simpler, and duplicates of files and programs are mostly removed.   When you install a program, its dependencies must be installed at the same time. Usually, most of the required dependencies will already be installed, but a few extras may be needed, too. So, when you install a package, don't be surprised if several other packages are installed too - these are just dependencies which are needed for your chosen package to function properly. |
| --- |

     four types of dependencies are specified using the following respective tags: 


* <buildtool\_depend>
* <build\_depend>
* <run\_depend>
* <test\_depend>

    A more realistic example that specifies build, runtime, and test dependencies could look as follows. 

```
1. <package>
2. <name>foo_core</name>
3. <version>1.2.4</version>
4. <description>
5. This package provides foo capability.
6. </description>
7. <maintainer email="ivana@willowgarage.com">Ivana Bildbotz</maintainer>
8. <license>BSD</license>
9. 
10. <url>http://ros.org/wiki/foo_core</url>
11. <author>Ivana Bildbotz</author>
12. 
13. <buildtool_depend>catkin</buildtool_depend>
14. 
15. <build_depend>message_generation</build_depend>
16. <build_depend>roscpp</build_depend>
17. <build_depend>std_msgs</build_depend>
18. 
19. <run_depend>message_runtime</run_depend>
20. <run_depend>roscpp</run_depend>
21. <run_depend>rospy</run_depend>
22. <run_depend>std_msgs</run_depend>
23. 
24. <test_depend>python-mock</test_depend>
25. </package>
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```


#### Metapackages




It is often convenient to group multiple packages as a single logical package. This can be accomplished through**metapackages**. A metapackage is a normal package with the following export tag in the package.xml: 为了方便，多个package可以group成单个的逻辑package。就用matapackage来实现。一个metapackage就是一个普通package带了下面的export tag 在它的package.xml里，就变成了metapackage。




```
 <export>
   <metapackage />
 </export>
```
 Other than a required 
<buildtool\_depends> dependency on 
catkin, metapackages can only have run dependencies on packages of which they group. 
  
 不需要有catkin作为buildtool depends, Metapackage可以只有run dependencies, 这些run dependencies就是它group的那些包，包们作这个metapackage的run\_dependencies即可。 
  

  
 另外，它的CMakeLists.txt也要有点不同，它有一个 required, boilerplate CMakeLists.txt file: 
 


```
cmake_minimum_required(VERSION 2.8.3)
project(<PACKAGE_NAME>)
find_package(catkin REQUIRED)
catkin_metapackage()
```
 Note: replace <PACKAGE\_NAME> with the name of the metapackage. 
  
 Metapackage now have CMakeLists.txt files after a discussion about installing the package.xml files: 
  
 https://groups.google.com/forum/#!msg/ros-sig-buildsystem/mn-VCkl2OHk/dUsHBBjyK30J 
  
 所以，现在尽管Metapackage的package.xml文件是被安装了，但是，有个要求是要强制满足的。这个要求就是，其他packages应该不要依赖metapackages。通过要求用户不要脱离了建议的 boilerplate CMakeLists.txt 代码来避免形成对metapackages的依赖。 
  

#### Additional Tags



* <url> - A URL for information on the package, typically a wiki page on ros.org.
* <author> - The author(s) of the package


#### package.xml就是这样了


   它就是关于Tags的文档，所以看看下面这篇文章即可。  
 


   http://wiki.ros.org/ROS/Tutorials/CreatingPackage  
    到Dependency Tags就完成了学习。最后的 package.xml 文件内容如下：



```
1. <?xml version="1.0"?>
2. <package>
3. <name>beginner_tutorials</name>
4. <version>0.1.0</version>
5. <description>The beginner_tutorials package</description>
6. 
7. <maintainer email="you@yourdomain.tld">Your Name</maintainer>
8. <license>BSD</license>
9. <url type="website">http://wiki.ros.org/beginner_tutorials</url>
10. <author email="you@yourdomain.tld">Jane Doe</author>
11. 
12. <buildtool_depend>catkin</buildtool_depend>
13. 
14. <build_depend>roscpp</build_depend>
15. <build_depend>rospy</build_depend>
16. <build_depend>std_msgs</build_depend>
17. 
18. <run_depend>roscpp</run_depend>
19. <run_depend>rospy</run_depend>
20. <run_depend>std_msgs</run_depend>
21. 
22. </package>
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  


Now that the  [package.xml](http://wiki.ros.org/catkin/package.xml), which contains meta information, has been tailored to your package, you are ready to move on in the tutorials. The[CMakeLists.txt](http://wiki.ros.org/catkin/CMakeLists.txt) file created by[catkin\_create\_pkg](http://wiki.ros.org/catkin/commands/catkin_create_pkg) will be covered in the later tutorials about building ROS code.



Now that you've made a new ROS package, let's 
[build our ROS package](http://wiki.ros.org/ROS/Tutorials/BuildingPackages). 
  

#### ROS Stack/ Package等各元件层级结构:


[Repository1]  
    [Stack1]  
        [Package1]  
                 [Nodes]  
                 [Messages]  
                 [Services]  
                 [Libraries]  
                 [Tools]  
         [/Package1]  
   
        [Package2]  
                 [Nodes]  
                 [Messages]  
                 [Services]  
                 [Libraries]  
                 [Tools]  
         [/Package2]  
    [/Stack1]  
    [Stack2]  
    ...  
    [/Stack2]  
 [/Repository1]  
 


  
 我的exbot帮我建立的ubuntu for ros版本是ros\_hydro版本，  
 在/opt/ros/hydro/bin目录下，可以看到有个rosstack命令，  
    $ rosstack list   
 可以列出本机已经安装的stacks， 我们还可以安装第三方 stacks完成想要的功能。  
   
 


#### 总结


**== 管理ROS环境变量 ==**   
 ref:  [Configure ROS workspace - Catkin & Rosbuild](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html)  
  1. Check the ROS env. variables like ROS\_ROOT/ROS\_PACKAGE\_PATH  
     $ export | grep ROS  
 -----------  
 declare -x ROSLISP\_PACKAGE\_DIRECTORIES="/home/exbot/catkin\_ws/devel/share/common-lisp"  
 declare -x ROS\_DISTRO="hydro"  
 declare -x ROS\_ETC\_DIR="/opt/ros/hydro/etc/ros"  
 declare -x ROS\_MASTER\_URI="http://localhost:11311"  
 declare -x ROS\_PACKAGE\_PATH="/home/exbot/catkin\_ws/src:/opt/ros/hydro/share:/opt/ros/hydro/stacks"  
 declare -x ROS\_ROOT="/opt/ros/hydro/share/ros"  
 declare -x ROS\_TEST\_RESULTS\_DIR="/home/exbot/catkin\_ws/build/test\_results"  
 -----------  
  2. source the setup.sh files  
     # source /opt/ros/<distro>/setup.bash  
  **== 建立ROS Workspace (catkin)==**   
 1. create a catkin workspace  
     $ mkdir -p ~/catkin\_ws/src  
     $ cd ~/catkin\_ws/src  
     $ catkin\_init\_workspace  
 2. Make  
     Even though the workspace is empty (there are no packages in the 'src' folder, just a single CMakeLists.txt link) you can still "build" the workspace:  
   
     $ cd ~/catkin\_ws/  
     $ catkin\_make  
   
 3. Source  
     $ source devel/setup.bash  
   
 4. Check:  
     $ echo $ROS\_PACKAGE\_PATH  
     /home/youruser/catkin\_ws/src:/opt/ros/indigo/share:/opt/ros/indigo/stacks  
 


5. Mixing catkin and rosbuild Workspaces  
    refer: "ROS by Example" 4.7  and ROS wiki:   [Overlaying](http://wiki.ros.org/catkin/Tutorials/workspace_overlaying)  
 


**=== 创建ROS Package (catkin) ===**   
 1. 确认你的catkin Workspace：~/catkin\_ws/  
 2. $ cd ~/catkin\_ws/src  
 3. 创建package  
    catkin\_create\_pkg <package\_name> [depend1] [depend2] [depend3] ... [dependn]  
 4. 编译catkin Workspace:  
    cd ~/catkin\_ws  
    catkin\_make  
 5. Source the generated setup file:  
    $ . ~/catkin\_ws/devel/setup.bash  
 6. 工具：  
    rospack  
    roscd <package\_name>  
 7. Customize Your Packages:  
    http://wiki.ros.org/ROS/Tutorials/CreatingPackage#ROS.2BAC8-Tutorials.2BAC8-catkin.2BAC8-CreatingPackage.Customizing\_Your\_Package  
 


  

===============


#### Reference about Overlaying - by YuanboShe



#### [overlay](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#id14)


再来思考一个问题：你有一个或者多个catkin workspace，同时你也有一个或者多个rosbuild workspace；而某个名字叫example\_package的package在ROS库，在每个workspace里面都存在，当你运行rosrun example\_package example\_package 的时候，到底会运行哪个位置的example\_package呢？


这就关系到overlay的概念。overlay就是一种操作，可以让不同的workspace层层覆盖，最底层的是ROS库。通过这种覆盖关系，当寻找某个package的时候，ROS会先从顶层的workspace找，如果找不到再依次往下找。也可以看成是一个路径链条，当我们想把通过overlay链接起来的workspace和ROS库的路径配入环境的时候，我们只需要source overlay链条顶层的workspace的setup.bash脚本即可。




### [overlay rosbuild\_ws->catkin\_ws->ROS库](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#id15)


最常见的配置是一个rosbuild workspace和一个catkin workspace了（对应的文件夹分别为 **rosbuild\_ws** 和**catkin\_ws** ）。因为有时候我们需要使用这两种workspace里面的package，而往往是自己的package优先于ROS库里面自带的package的。



#### [overlay catkin\_ws->ROS库](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#id16)




```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
cd ~/catkin_ws/
catkin_make

```


第一条语句创建catkin\_ws文件夹，以及src文件夹；第二条和第三条语句是在src路径下初始化catkin workspace，并指定src路径是package存放的路径；第四条和最后一条语句是在catkin\_ws路径下编译整个workspace，将会生成catkin workspace的结构，以及编译src下所有的package。



Note


上面的操作会自动overlay ROS库，即catkin\_ws->ROS库。如果不需要再加入rosbuild workspace，则执行echo "source ~/catkin\_ws/devel/setup.bash" >>~/.bashrc 就行了，跳过后面的所有步骤。





#### [overlay rosbuild\_ws->catkin\_ws](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#id17)




```
sudo apt-get install python-rosinstall

```


在新装的系统第一次运行rosbuild相关操作前，还要先安装一下rosinstall。




```
mkdir ~/rosbuild_ws
cd ~/rosbuild_ws
rosws init . ~/catkin_ws/devel
mkdir ~/rosbuild_ws/sandbox
rosws set ~/rosbuild_ws/sandbox

```


前两条语句创建并进入rosbuild\_ws文件夹；第三条语句初始化rosbuild\_ws并overlay前面创建的catkin workspace；第四条和最后一条语句创建文件夹sandbox并将其设置为package存放的文件夹。




#### [source setup.bash](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#id18)




```
echo "source ~/rosbuild_ws/setup.bash" >> ~/.bashrc
source ~/.bashrc

```


每次打开terminal，都将会执行 *~/.bashrc* 脚本。因此，第一条语句将引号内的内容写入 *~/.bashrc* 后，每次打开terminal，overlay顶层的rosbuild\_ws的setup.bash都会被source，就不用我们手动source了。




#### [检查overlay路径](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#id19)




```
echo $ROS_PACKAGE_PATH

```


确认是否显示了下面4个路径：



> /home/<user>/rosbuild\_ws/sandbox:/home/<user>/catkin\_ws/src:/opt/ros/<distro>/share:/opt/ros/<distro>/stacks


如果不全，说明之前的overlay出了问题。再进一步做检查：




```
gedit ~/rosbuild_ws/.rosinstall

```


看看有没有下面的内容：



```
- setup-file: {local-name: /home/<user>/catkin_ws/devel/setup.sh}
- other: {local-name: sandbox}

```

第一条指被overlay的catkin workspace路径，第二条指该rosbuild workspace的package存放目录。


更多的overlay概念，可以参考  [[catkin\_workspace\_overlaying]](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#catkin-workspace-overlaying) 和  [[using\_rosbuild\_with\_catkin]](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#using-rosbuild-with-catkin) 。




### [Reference](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#id20)




| [[catkin\_conceptual\_overview]](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#id3) | <http://wiki.ros.org/catkin/conceptual_overview> |
| --- | --- |




| [[catkin\_workspace\_overlaying]](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#id6) | <http://wiki.ros.org/catkin/Tutorials/workspace_overlaying> |
| --- | --- |




| [[using\_rosbuild\_with\_catkin]](http://my.phirobot.com/blog/2013-12-overlay_catkin_and_rosbuild.html#id7) | <http://wiki.ros.org/catkin/Tutorials/using_rosbuild_with_catkin> |
| --- | --- |


  


Compile and Use the ROS Source Code from Git:


![](https://img-my.csdn.net/uploads/201605/11/1462958472_6646.jpg)


  
 



### 一个比较好的ROS教学大纲：


![](https://img-blog.csdn.net/20160620175255233?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
  


  
     
 


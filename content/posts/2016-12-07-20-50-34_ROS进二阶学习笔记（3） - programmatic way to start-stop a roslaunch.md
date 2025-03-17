---
layout: post
title:  "ROS进二阶学习笔记（3） - programmatic way to start-stop a roslaunch"
date: 2016-12-07 20:50:34 +0800
tags: 
slug: p20161207205034
---

# ROS进二阶学习笔记（3） - programmatic way to start-stop a roslaunch





## ROS进二阶学习笔记（3） - programmatic way to start/stop a roslaunch


Sometimes, we need to start/stop a ros .launch file/ ros [node](https://so.csdn.net/so/search?q=node&spm=1001.2101.3001.7020) in a programmatic way, especially when we bring in the SMACH method to handle our applications.  
 Here are some threads in answers.ros.org web, and I summarised.  
 


Just share what I've done. to be Generous. Specify this source if you need to repost/quote. thx~
  

### First of first:

 You have two requirments: Starting a node or a launch file: 
  
 ----------------------------------------------------------- 
  

  

### To Start a Node:

    1. roslaunch api: 
  
    http://wiki.ros.org/roslaunch/API%20Usage 
  

  
 ========================== 
  

### To Start a Launch file:


#### === 1. subprocess ===


  subprocess module may be the first choose to start/stop a roslaunch process in programmatic way.   
   The  [`subprocess`](https://docs.python.org/2/library/subprocess.html#module-subprocess "subprocess: Subprocess management.") module allows you to spawn new processes, connect to theirinput/output/error pipes, and obtain their return codes. This module intends toreplace several older modules and functions:





```
os.system
os.spawn*
os.popen*
popen2.*
commands.*

```


   ref: https://docs.python.org/2/library/subprocess.html 
  
          http://www.bogotobogo.com/python/python\_subprocess\_module.php 
  
          http://www.cnblogs.com/vamei/archive/2012/09/23/2698014.html (CN) 
  
          http://hackerxu.com/2014/10/09/subprocess.html (CN) 
  
   
  
 ---------------------- 

   rather than the `execfile` runs a python file, the `subprocess.call` execute roslaunch and give it either absolute path to


  
 your launch file or two parameters where the first is the package of your launch file and the second is the name of the file. 
  
 (http://answers.ros.org/question/42849/how-to-launch-a-launch-file-from-python-code/) 
  
     
  
    the subprocess module can also be used to terminated processes, as described in this stackoverflow question. 
  
 (https://stackoverflow.com/questions/12321077/killing-a-script-launched-in-a-process-via-os-system) 
  

  
    User tfoote said:"pass the SIGINT to whatever subprocess handle you started it with in code." 
  
    ref: http://answers.ros.org/question/10493/programmatic-way-to-stop-roslaunch/ 
  

  

#### === 2. os.system('roslaunch pkg \*.launch') ===

    #process gets stucked when this line run 
  
    You can pack a list of code into a process which can be launched by os.system 
  
    You may need to usd subprocess to terminate it: 
  
 (https://stackoverflow.com/questions/12321077/killing-a-script-launched-in-a-process-via-os-system) 
  

  

  

#### ===\*3. "roslaunch.parent.ROSLaunchParent" object ===

 >>  import roslaunch 
  
 >> 
  
 >>  uuid = roslaunch.rlutil.get\_or\_generate\_uuid(None, False) 
  
 >>  roslaunch.configure\_logging(uuid) 
  
 >>  launch = roslaunch.parent.ROSLaunchParent(uuid, [file\_path]) 
  
 >>  launch.start() 
  
 >>  launch.shutdown() 
  
 (http://answers.ros.org/question/42849/how-to-launch-a-launch-file-from-python-code/) 
  

  

#### === 4. roslaunch.main ===

 User lucasw said: 
  
 >>  import roslaunch; 
  
 >>  args = ['roslaunch', 'my\_package', 'foo.launch']; 
  
 >>  roslaunch.main(args) 
  

  
     - but it blocks and doesn't have the stop/is\_alive methods. 
  
 ref: (http://answers.ros.org/question/12260/terminate-launch-other-node-from-code/) 
  
     
  
 also, in this thread: 
  
 User lucasw said: 
  
 Unless the scriptapi actually can run launch files, here is my (somewhat confusing) solution. 
  
 1-Create a node that uses scriptapi to start/stop a single node specified by params. (the scriptapi approach doesn't look like it can run launch files, just nodes- it is just a programmatic rosrun, not roslaunch?) 
  

  
 2-Create a node that uses roslaunch directly to start a param defined launch file, with a hack to get around the way roslaunch uses sys.argv. The main method blocks so doesn't have a nice start/stop/is\_alive interface like scriptapi. 
  
     >> sys.agv=['roslaunch', 'pkg\_name', 'foo.launch', 'arg:=bar'] 
  
     >> import roslaunch 
  
     >> roslaunch.main(args) 
  
 3-Start the second roslaunch raw node within the first scriptapi node, and then if the stop method is used it uses the scriptapi stop method which interrupts the roslaunch, kills all the child nodes as desired. 
  
 ref(http://answers.ros.org/question/10493/programmatic-way-to-stop-roslaunch/) 
  

  

  
 ============================== 
  

### Stop the launch file:


  

#### 1. Python:

 >> import os 
  
 >> os.system('roslaunch pkg launch.launch')  #The code get stuck(blocked) when this line launched. 
  

  
    in the launch file make one node has the tag of the requied=true, so all the other nodes started by that launch file will 
  

  
 halt when that node is killed. 
  
    e.g. <node pkg="rosbag" type="play" name="rosbagplay" args="test.bag" required="true"/> 
  
 --- 
  
    bash = rosnode kill rplidarNode 
  
    Equivalent python code: 
  
 >> import os 
  
 >> os.system('rosnode kill rplidarNode') 
  

  
    About the block problem, the user tanghz posted this:"remember to new a thread, or the code would block and you cannot stop the node." I do not understand the "new a thread". ref:(http://answers.ros.org/question/223090/start-or-stop-the-ros-node-in-another-node/) 
  

  

#### 2. Python/C++ :

    User tfoote said:"pass the SIGINT to whatever subprocess handle you started it with in code." 
  
    ref: http://answers.ros.org/question/10493/programmatic-way-to-stop-roslaunch/ 
  

  
 ---------------------------------------------------------- 
  

### Some other ways that I didn't try to test or look into it:


  
 X. C++: 
  
    >> system("roslaunch rplidar\_ros rplidar.launch");  //start 
  
    >> 
  
    >> system("rosnode kill rplidarNode");              //stop 
  

  
    By the way, remember to new a thread, or the code would block and you cannot stop the node. 
  
    ref: http://answers.ros.org/question/223090/start-or-stop-the-ros-node-in-another-node/ 
  

  
 User supoerjax said:" use system("pkill roslaunch") in your node to kill the roslaunch from the system level." 
  
 ref:(http://answers.ros.org/question/10493/programmatic-way-to-stop-roslaunch/) 
  

  
 XI. C++/Bash: 
  
     ref:http://answers.ros.org/question/12260/terminate-launch-other-node-from-code/ 
  
     ref: 
  

  
 XII. `ros::kill(\_\_pid\_t \_\_pid, int \_\_sig)` and `getpid()` s 
  
 in that thread they also discussed about the SIGINT/Ctrl-C methods to stop the roslaunch 
  
 ref:(http://answers.ros.org/question/10493/programmatic-way-to-stop-roslaunch/) 
  

  
 


### references


\* http://answers.ros.org/question/10493/programmatic-way-to-stop-roslaunch/  
 \* http://answers.ros.org/question/223090/start-or-stop-the-ros-node-in-another-node/  
 http://answers.ros.org/question/203154/is-it-possible-to-run-and-quit-launch-files-from-code-inside-a-node/  
 http://answers.ros.org/question/12260/terminate-launch-other-node-from-code/  
 http://answers.ros.org/question/42849/how-to-launch-a-launch-file-from-python-code/  
 





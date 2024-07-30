---
layout: post
title:  "ROS进阶学习手记 6 -- Configure ROS Network - 利用PC1控制PC2上的乌龟"
date: 2015-07-21 15:16:04 +0800
tags: 
slug: p20150721151604
---

# ROS进阶学习手记 6 -- Configure ROS Network - 利用PC1控制PC2上的乌龟





Configure the ROS Networks: Quick Reference: <http://blog.csdn.net/sonictl/article/details/46986565#t4>  
 Above is for the higher user's reference.  
 


  
 


  
 


  
 


  
 


Below is the tutorial for the beginners:  
 


在完成了上一节，我们已经可以自己写node向某个topic发送某个msg data structure的Messages。


这一节我想完成 Remote 通信的任务，真正体现ROS的分布式计算的特点。


  
 参考：   
 


   http://wiki.ros.org/ROS/NetworkSetup  
    http://wiki.ros.org/ROS/Tutorials/MultipleMachines  
 


#### 网络连接：


    物理连接：  
 


![](https://img-blog.csdn.net/20150723113154468?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 



   使用Bridge方式桥接，相当于现在有了4台电脑用网线连接起来。（怎么建立见文后附录）


* 配置虚拟机1的ip为：  
         IP addr:192.168.2.111    Mask:255.255.255.0  DefaultGateway: 192.168.2.1   Bcast:default
* 配置宿主机1的ip为：  
         IP addr:192.168.2.1       Mask:255.255.255.0  DefaultGateway: 192.168.2.1   Bcast:default
* 配置虚拟机2的ip为：  
         IP addr:192.168.2.222   Mask:255.255.255.0  DefaultGateway: 192.168.2.1   Bcast:default
* 配置宿主机2的ip为：  
         IP addr:192.168.2.2       Mask:255.255.255.0  DefaultGateway: 192.168.2.1   Bcast:default


  
 好，我们假设建立了ubt - ubt之间的连接（怎么建立见文后附录，或http://blog.csdn.net/sonictl/article/details/47005273） 
  

我们在ubt\_for\_ROS1上ping 192.168.2.222,  在ubt\_for\_ROS2上ping 192.168.2.111, 都能ping通。


用netcat测试端口是否连通：http://wiki.ros.org/ROS/NetworkSetup#Further\_check:\_netcat  
 


    在ubt\_for\_ROS1上：   
 



```
           $ netcat -l 1431

```

    在ubt\_for\_ROS2上：   
 



```
           $ netcat 192.168.2.111 1431

```
 此时在俩ubt\_for\_ROS上的Terminal里，敲入任何字符，回车，都能发送到对方，正如：you will be able to type back and forth between the two consoles, like an old-fashioned chat program. 
  


反过来再测试一遍~  OK。



#### === ROS 对 名称 的解析 ===

   当一个ROS节点广播一个topic时，它会提交 hostname:port 组合（URI），别的nodes想要收听这个topic时，会用到这个URI。所以一个node提供的hostname一定要被所有别的nodes用得上，才能与那个topic联系。ROS客户端的库使用了这个名字，这个名字是机器报告的它的hostname。这个hostname 就是在Terminal里运行命令hostname返回的那个值。 
  
   所以我们不仅要改hosts文件，使机器访问的 机器名 与 ip对应上，我们还需要修改hostname。 
  
   这里我们两台ubuntu for ros的hostname都是ubuntu， 
  
   >>把ubt\_for\_ROS1的hostname改为： 
  
      ubtros1 
  
   >>把ubt\_for\_ROS2的hostname改为： 
  
      ubtros2 
  
 在ubt\_for\_ROS1上 
  


```
  $sudo gedit /etc/hostname
```
   把“ubuntu”改为“ubtros1” 
  
 在ubt\_for\_ROS2上 
  


```
  $sudo gedit /etc/hostname
```

  把“ubuntu”改为“ubtros2” ，这样就区分开了俩ubt\_for\_ROS


修改hosts文件：  
 



```
  $ sudo vi /etc/hosts

```


```
    #至于vi工具的使用方法，请自行搜索
    #添加如下的ip解析规则
    192.168.2.111   ubtros1
    192.168.2.222   ubtros2
```
After you are finished configuring your networking files, don't forget to restart your network for the changes to take effect. 或者直接重启ubuntu更保险。 
  


```
           exbot@ubuntu:~$ sudo /etc/init.d/networking restart 
           * Running /etc/init.d/networking restart is deprecated because it may not enable again some interfaces
           * Reconfiguring network interfaces...                                   [ OK ]         
```

背景知识：




```
修改Ubuntu主机名：
-$sudo vi /etc/hostname

修改hosts:
-$sudo vi /etc/hosts

#据说只改hostname不改hosts会出现问题。
#其实是hostname是主机名，hosts是DNS转发ip用的一个查询表而已。

hostname命令不带任何参数默认是用来显示主机名的，如果在该命令的后面加上一个字符串，那么这个字符串就是暂时设置主机名，重启后复原。

```

好了，我就改了hosts和hostname。检查后继续！ping ubtros1 和 ping ubtros2都是可以的。hostname命令运行结果也是变了的。 
  
 参考: my "/etc/hosts" file: 
  


```
127.0.0.1	localhost
127.0.1.1	ubuntu
192.168.1.103   ubtros1
192.168.1.102   ubtros2

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

  



在俩虚拟机上都配置相同的环境变量：（有时候提示连接不上ros master, 检查环境变量）




```
      $ export ROS_MASTER_URI=http://ubtros1:11311
```

检查环境变量：



```
      $ echo $ROS_MASTER_URI

```

**这里我要补充的是：** ROS 在启动 roscore 时调用的 hostname 可能涉及到多个环境变量： $ROS\_HOSTNAME, $ROS\_MASTER\_URI 这俩环境变量又和 hosts & hostname的值有关系。请不要设置错了。具体详细设置方法的可以参考： 《ROS by Example for Hydro Vol1》书中4.12 Network Between a Robot and a Desktop Computer  
 简而言之：$ROS\_MASTER\_URI 要统一。尽管HOSTNAME可以各设各的，但roscore启动时会引用本机的ROS\_HOSTNAME，所以roscore应该在MASTER机器上启动。  
 


#### 测试：开始运行我们的nodes在俩虚拟机上!


  在虚拟机“ubt\_for\_ROS1”上：   
 



```
           $ roscore
```
   此时会显示ROS\_MASTER\_URI=ubtros1:11311, 切记，是ROS\_MASTER\_URI= 
ubtros1:11311 
  
 在虚拟机“ubt\_for\_ROS2”上： 
  


```
           $ rosrun turtlesim turtlesim_node
```
 此时会在虚拟机“ubt\_for\_ROS2”上出现乌龟的窗口，说明已经连接上“ubt\_for\_ROS1”上的ros master！ 

在虚拟机“ubt\_for\_ROS1”上：   
 



```
           $ rosrun robot_cleaner robot_cleaner_node
```

没动，在在虚拟机“ubt\_for\_ROS2”上，$ rostopic list ： unable to communicate with ros master ,重新配置了一下ROS\_MASTER\_URI，解决。


**在ubt\_for\_ROS1 屏幕截图：**


     ![](https://img-blog.csdn.net/20150723110737119?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


**在ubt\_for\_ROS2 屏幕截图：**


     ![](https://img-blog.csdn.net/20150723110923668?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


![大笑](http://static.blog.csdn.net/xheditor/xheditor_emot/default/laugh.gif)  
 


#### **附录：==== 俩虚拟机的网络通信的配置====**


关于网线直连的配置：http://jingyan.baidu.com/article/3065b3b6c552edbecff8a4a1.html  
 


双方Host\_PC:Win7都能ping通对方Win7。


关于虚拟机的网络配置方式，参考：http://blog.csdn.net/mrjy1475726263/article/details/7772372


使用Bridge方式桥接，相当于现在有了4台电脑用网线连接起来。


* 配置虚拟机1的ip为：  
         IP addr:192.168.2.111    Mask:255.255.255.0  DefaultGateway: 192.168.2.1   Bcast:default
* 配置宿主机1的ip为：  
         IP addr:192.168.2.1       Mask:255.255.255.0  DefaultGateway: 192.168.2.1   Bcast:default
* 配置虚拟机2的ip为：  
         IP addr:192.168.2.222   Mask:255.255.255.0  DefaultGateway: 192.168.2.1   Bcast:default
* 配置宿主机2的ip为：  
         IP addr:192.168.2.2       Mask:255.255.255.0  DefaultGateway: 192.168.2.1   Bcast:default


相关截图：


     宿主机1的设置：  
      ![](https://img-blog.csdn.net/20150722161618494?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
   
      ![](https://img-blog.csdn.net/20150722161636701?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
   
     虚拟机1相关设置：


     ![](https://img-blog.csdn.net/20150722161658187?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 


现在费了这么大劲，解决了连接问题，当然还有别的方式，比如一台宿主机两台虚拟机，读者去自己调研。


只要两台虚拟机之间能ping通即可。  
 


  
 


  
 


### ROS Networking Between a Robot and Desktop Computer/Controller


                  by sonictl ref:book 'ROS by Example\_Vol1'  
 ROS requires the machines are in one domain network. i.e. Machine\_A and Machine\_B can ping each other.  
  \_\_\_\_\_\_\_\_\_\_\_\_\_                \_\_\_\_\_\_\_\_\_\_\_\_\_  
 |  (Desktop)  |              |   (Robot)   |  
 |  Machine\_A  |<--- Ping --->|  Machine\_B  |  
 |\_\_\_\_\_\_\_\_\_\_\_\_\_|              |\_\_\_\_\_\_\_\_\_\_\_\_\_|  
  by: http://blog.csdn.net/sonictl  
   
 Let's see the configuration steps necessary for a working ROS network:  
   
 **Step1: Configure the IP address for M\_A and M\_B**  
   Manually setting the static IP address as below:  
   >>IP for M\_A: 192.168.1.101  
   >>IP for M\_B: 192.168.1.201  
   
 **Step2: Configure the hostname of each machine**  
   The hostname is the ID/mark for the machine be identified by the domain network.  
   On M\_A and M\_B, modify the file: /etc/hostname by this command:  
   $ sudo gedit /etc/hostname  
     
   >> hostname for M\_A: my\_desktop  
   >> hostname for M\_B: my\_robot  
   
 **Step3: Configure the hosts mapping table of each machine**  
   The hosts mapping table is mapping of IP to the machine's hostname/ID/mark.   
   the mapping table is to be identified by the domain network.  
   Add all the hostname/ID/mark <--> IP mappings for the machines in the network, so that they know each other's IP.  
   On M\_A and M\_B, modify the file: /etc/hosts by this command:  
   
   $ sudo gedit /etc/hosts  
     
   >> for M\_A, add this in its hosts file:  
      192.168.1.101 my\_desktop  
      192.168.1.201 my\_robot  
        
   >> for M\_B, add this in its hosts file:  
      192.168.1.101 my\_desktop  
      192.168.1.201 my\_robot  
   
   Here you can test this: On M\_A, ping M\_B's hostname. vice versa.  
   http://blog.csdn.net/sonictl  
   
 **Step4: Time Synchronization**  
   
   Time synchronization between machines is often critical in a ROS network since frame  
   transformations and many message types are timestamped. An easy way to keep your  
   computers synchronized is to install the Ubuntu chrony package on both your desktop  
   and your robot. This package will keep your computer clocks synchronized with  
   Internet servers and thus with each other.  
   
   To install chrony, run the command:  
   $ sudo apt-get install chrony  
   
   After installation, the chrony daemon will automatically start and begin synchronizing  
   your computer's clock with a number of Internet servers.  
   
 **Step5: Setting the ROS\_MASTER\_URI and ROS\_HOS TNAME Variables**  
   These two environmental variables allocates the master and slaves of one ROS network.  
   One ROS network can and have to have one Master.  
   Modify the ~/.bashrc file to allocates the master and ROS\_HOSTNAME variables.  
   $ sudo gedit ~/.bashrc  
   
   On M\_A: (SLAVE), add these lines below to its ~/.bashrc file:  
   >> export ROS\_HOSTNAME=my\_desktop  
   >> export ROS\_MASTER\_URI=http://my\_robot:11311  
   Save and exit the ~/.bashrc file.  
   
   On M\_B: (Master), add these lines below to its ~/.bashrc file:  
   >> export ROS\_HOSTNAME=my\_robot  
   >> export ROS\_MASTER\_URI=http://my\_robot:11311  
   Save and exit the ~/.bashrc file.  
   
   Close all the terminals and reopen, check the variables by this commands:  
   $ echo $ROS\_HOSTNAME  
   $ echo $ROS\_MASTER\_URI  
   
 **Step6: Test (blog.csdn.net/sonictl)**  
   You can use ssh to login to the other machine:  
   $ ssh <user\_name>@<hostname of the other machine>  
   
   If you can log into it. you can run commands on the other machine through ssh tool.  
     
   Example:http://blog.csdn.net/sonictl  
   on M\_A, open a terminal(Ctrl+Alt+T):  
   $ ssh <usrname>@my\_robot  
   $ <Enter Password>  
   $ roscore  
   
   http://blog.csdn.net/sonictl  
   on M\_A, open another terminal(Ctrl+Alt+T):  
   $ rostopic list  
   You can get:  
   /rosout  
   /rosout\_agg  
   That means you successfully configured the ROS network.  
   
 Good Luck! http://blog.csdn.net/sonictl  
   
 





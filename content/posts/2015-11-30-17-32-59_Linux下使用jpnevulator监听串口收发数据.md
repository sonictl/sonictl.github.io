---
layout: post
title:  "Linux下使用jpnevulator监听串口收发数据"
date: 2015-11-30 17:32:59 +0800
tags: 
slug: p20151130173259
---

# Linux下使用jpnevulator监听串口收发数据





 ref: 
<http://unix.stackexchange.com/questions/12359/how-can-i-monitor-serial-port-traffic>
  
 Serial Port Traffic/Data Monitor for Linux 
  
 Linux下可以监听串口上数据的应用 
  

## [Linux](https://so.csdn.net/so/search?q=Linux&spm=1001.2101.3001.7020)下使用jpnevulator监听串口收发数据


The serial sniffer/monitor for linux:


There is port monitoring tool to watch the packets written on the port. Especially when you want to check if your program written works. Tool to see if your application is sending the messages to the port.


  
 


### OK, Let's get started!


The opensource   
 


* app: jpnevulator
* official site:   <https://jpnevulator.snarl.nl/>
* Download from my share:  [点击打开链接](http://download.csdn.net/detail/sonictl/9305249) or source link: https://jpnevulator.snarl.nl/src


### 基本使用：


命令：




```
$ jpnevulator --ascii --timing-print --tty /dev/ttyUSB0:mySerial --read 
```
 这会接收发送到 /dev/ttyUSB0 上的数据，并显示出来。但是原先接收此数据的设备就被take over取代了。 



```
$ jpnevulator --ascii --pty=:SerialSent --pass --tty "/dev/ttyUSB0:SerialReceived" --rea
```
 --pty 会首先虚拟一个假的终端/dev/pts/4出来，--pass会把/dev/pts/4上接收到的数据转发到/dev/ttyUSB0 

--read会读取/dev/pts/4收到的和/dev/ttyUSB0从外部收到的，并显示出来。  
   
 


### 实验设计 Let's have test


主机与从机通过串口连接，设置同样的波特率。  
 


主机上运行：  
 $ jpnevulator --ascii --pty=:SerialSent: --pass --tty "/dev/ttyUSB0:SerialReceived:" --read  
 得到：jpnevulator: slave pts device is /dev/pts/4.  
   
 主机打开cutecome工具，选择/dev/pts/4 （与上面生成的 psuedo terminal ID 一致），发送：Sent from MSTr  
 可见从机上显示：Sent from MSTr.  
 主机上jpnevulator显示：  
 SerialSent:  
 53 65 6E 74 20 66 72 6F 6D 20 4D 53 54 72 0A       Sent from MSTr.  
   
   
 从机打开cutecom工具，发送：Sent from SLAV  
 可见主机cutecom工具显示：Sent from SLAV.  
 主机上jpnevulator显示：  
 SerialReceived:  
 53 65 6E 74 20 66 72 6F 6D 20 53 4C 41 56 0D 0A    Sent from SLAV..  
 ![](https://img-blog.csdn.net/20151130173648047?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
 ============  
 Enjoy!  
 


  
 




文章知识点与官方知识档案匹配，可进一步学习相关知识[CS入门技能树](https://edu.csdn.net/skill/gml/gml-1c31834f07b04bcc9c5dff5baaa6680c?utm_source=csdn_ai_skill_tree_blog)[Linux入门](https://edu.csdn.net/skill/gml/gml-1c31834f07b04bcc9c5dff5baaa6680c?utm_source=csdn_ai_skill_tree_blog)[初识Linux](https://edu.csdn.net/skill/gml/gml-1c31834f07b04bcc9c5dff5baaa6680c?utm_source=csdn_ai_skill_tree_blog)40632 人正在系统学习中

---
layout: post
title:  "在Fedora14中安装 Virtual Box Guest Additions"
date: 2014-05-04 16:35:59 +0800
tags: 
slug: p20140504163559
---

# 在Fedora14中安装 Virtual Box Guest Additions





### 在Fedora14中安装 Virtual Box Guest Additions


文章来源：<http://www.hackourlife.com/build-install-virtualbox-vbox-guest-additions-in-fedora-14/>


##### 写在前面


    楼主遇到的问题，yum 相关指令出现错误：Cannot retrieve repository metadata (repomd.xml) for repository fedora. Please verify ...


    这个问题是yum源问题。楼主因为**Fedora没有连接上Internet**所以导致这样。相关的原因还可以百度之。


    以下是如何安装vBox(virtual Box) Guest Additions 在Fedora14下的例程：



#### Build / Install VirtualBox (vbox) Guest Additions in Fedora 14


* Wednesday, November 17, 2010,








  



  






Installing the guest additions for Linux guests can be a hassle for VirtualBox. The steps involved for Fedora 14 Laughlin is as follows (this installation used Fedora 14 Live CD and VirtualBox 3.2.10)
 [![](http://www.hackourlife.com/wp-content/uploads/2010/11/Fedora-14_021-600x450.jpg)](http://www.hackourlife.com/wp-content/uploads/2010/11/Fedora-14_021.jpeg)






 1. Install kernel headers for the kernel you are running with, as a super user (root) do the following



> ```
> `yum install kernel-devel-$(uname -r)`
> ```


 2. Install the dependencies and gcc-compiler



> ```
> yum -y install dkms gcc
> ```


 3. Find the kernel version that you are running by



> ```
> uname -r
> ```


 Use the output you get to set the kernel path variable



> ```
> export KERN_DIR=/usr/src/kernels/output_from_above/
> ```


 Now run the installer (still continuing to be super user) based on the version of your Linux guest (32 or 64 bit) and the guest additions should install just fine.



> ```
> cd /media/VBOXADDITIONS_3.2.6_63112
> ```
> 
> 
> ```
> ./VBoxLinuxAdditions-x86.run
> ```



安装完成以后可以使用共享文件夹share folder with host windows PC:


[Ubuntu VirtualBox中实现文件夹共享](http://blog.csdn.net/wyzxk888/article/details/5961099): <http://blog.csdn.net/wyzxk888/article/details/5961099>  
 




```
##### 2. 设置共享文件夹


  重启完成后在虚拟机管理UI上（Vbox、VMware），添加一个共享文件夹(E:\DevelopTools)，选项固定和临时是指该文件夹是否是持久的。共享名可以任取一个自己喜欢的，此时是DevelopTools，尽量使用英文名称。

  3. 挂载共享文件夹

  重新进入虚拟Ubuntu，在命令行终端下输入：

  sudo mkdir /mnt/myshared
  sudo mount -t vboxsf DevelopTools /mnt/myshared


  其中"DevelopTools"是之前创建的共享文件夹(E:\DevelopTools)的名字。OK，现在虚拟机上的系统和主机可以互传文件了。

  假如您不想每一次都手动挂载，可以在/etc/fstab中添加一项

  
```
DevelopTools /mnt/myshared vboxsf rw,gid=100,uid=1000,auto 0 0
```
  (注意：因为fstab是只读文件，所以可以采用  cd /etc  sudo vi fstab  来编辑添加这命令  )  这样就能够自动挂载了。  4. 卸载的话使用下面的命令：  sudo umount -f /mnt/myshared  注意：  共享文件夹的名称千万不要和挂载点的名称相同。比如，上面的挂载点是/mnt/myshared，如果共享文件夹的名字也是myshared的话，在挂载的时候就会出现如下的错误信息(看<http://www.virtualbox.org/ticket/2265>)：  /sbin/mount.vboxsf: mounting failed with the error: Protocol error二、主机为Linux时：1.在VirtualBox中设备--分配数据空间--添加2.然后起个名字（myyshare），选择linux上的路径3. 设置刚才设置好的共享文件夹，即，在虚拟机中的网络上右键，选择映射网络驱动器 注意，文件夹位置不要选择-浏览，手动写好路径重要重要！\\vboxsvr\共享目录名                例如：\\vboxsvr\myyshare
```

  



  
 





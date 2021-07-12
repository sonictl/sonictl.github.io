---
layout: post
title: 'win7与VMware/VBox下linux共享文件夹方法'
date: 2013-01-04 22:01:00 +0800
category: from_cnblogs
slug: p20130104220100
---


在Windows7系统环境下，用vmware安装好Ubuntu10.04系统后，Ubuntu默认是安装有Vmware Tools的。在这种情形下，有两种方法来共享Win7系统下的文件夹与文件。<br>
<br>
采用“拖拉”方式。Vmware很智能，可以直接将Win7下的文件拖进Ubuntu系统里。<br>
采用“共享文件夹”方式。首先，在Vmware下，设定好Win7的共享文件夹路径。然后，在Ubuntu系统下，执行: sudo vmware-config-tools.pl<br>
一般需要安装linux-header，执行命令如下：<br>
sudo apt-get install linux-headers-`uname -r` <br>
最后，在Ubuntu的/mnt/hgfs/文件夹下就可以看见Win7的文件了。<br>
<br>
<br>
如果使用secureCRT连接不到你的虚机，<br>
<br>
Server版需要安装ssh-server，命令：sudo apt-get install openssh-server ，下次重启需要手动启动：sudo/etc/init.d/ssh start<br>
<div style="color:rgb(51,51,51); font-family:Arial; font-size:14px; line-height:26px">
<br>
</div>
<div style="color:rgb(51,51,51); font-family:Arial; font-size:14px; line-height:26px">
<img alt="微笑" src="http://static.blog.csdn.net/xheditor/xheditor_emot/default/smile.gif"><br>
<br>
<div id="sina_keyword_ad_area2" class="articalContent   ">
<div>----------------------------------------</div>
<div>WIN7下Virtualbox虚拟Ubuntu, 共享文件夹设置</div>
<div>----------------------------------------</div>
<div>sudo mount -t vboxsf games /mnt/share</div>
<div><br>
</div>
<div>找了好久找到一个比较完善的共享文件夹的方法 希望对大家有用&nbsp;<wbr></div>
<div>我ubuntu是新氧的ubuntu 9.04，sun vitualbox/ Ubuntu 12.04官方iso也亲测过。</div>
<div><br>
</div>
<div>1. 安装增强功能包(VBoxGuestAdditions)</div>
<div><br>
</div>
<div>打开虚拟机，设置ubuntu9.04，找到光驱选项加载VBoxGuestAdditions。iso.（该镜像就在虚拟机的安装目录下面），确定</div>
<div><br>
</div>
<div>运行ubuntu，在光驱下就会有VBoxGuestAdditions镜像，打开镜像，运行autorun.sh,系统就会自动安装,安装完毕后会提示要重启Ubuntu。</div>
<div><br>
</div>
<div>2. 设置共享文件夹</div>
<div><br>
</div>
<div>有两种设置共享文件夹的方法,1运行Ubuntu前对其进行设置,打开设置选项-数据空间,右边有加载文件夹选项,加载一个共享文件夹,比如D:\games,确定</div>
<div>2 在Ubuntu已经运行时加载,在Ubuntu界面的右下角有一个文件夹选,右击可以加载</div>
<div><br>
</div>
<div>3. 挂载共享文件夹</div>
<div><br>
</div>
<div>重新进入虚拟Ubuntu，在命令行终端下输入：</div>
<div><br>
</div>
<div>sudo mkdir /mnt/shared</div>
<div><br>
</div>
<div>sudo mount -t vboxsf games /mnt/shared</div>
<div><br>
</div>
<div>其中&quot;games&quot;是之前创建的共享文件夹的名字。</div>
<div><br>
</div>
<div>注意：当虚拟机linux系统关机以后，下次使用需要重新挂载。OK，现在Ubuntu和主机可以互传文件了。</div>
<div><br>
</div>
<div>&nbsp;<wbr> &nbsp;<wbr>假如您不想每一次都手动挂载，可以在/etc/fstab中添加一项</div>
<div><br>
</div>
<div>&nbsp;<wbr> &nbsp;<wbr> games /mnt/shared vboxsfrw,gid=100,uid=1000,auto 0 0</div>
<div><br>
</div>
<div>&nbsp;<wbr> &nbsp;<wbr> 这样就能够自动挂载了。</div>
<div><br>
</div>
<div>&nbsp;<wbr> &nbsp;<wbr> 或者你<span style="line-height:21px">虚拟机linux系统不关机，关闭虚拟机的时候，选“save the machinestate.”</span></div>
<div><br>
</div>
<div>4. 卸载的话使用下面的命令：</div>
<div><br>
</div>
<div>sudo umount -f /mnt/shared</div>
<div><br>
</div>
<div>注意：</div>
<div><br>
</div>
<div>共享文件夹的名称千万不要和挂载点的名称相同。比如，上面的挂载点是/mnt/shared，如果共享文件夹的名字也是shared的话，在挂载的时候就会出现如下的错误信息(看http://www.virtualbox.org/ticket/2265)：</div>
<div><br>
</div>
<div>/sbin/mount.vboxsf: mounting failed with the error: Protocolerror</div>
</div>
<br>
</div>
   

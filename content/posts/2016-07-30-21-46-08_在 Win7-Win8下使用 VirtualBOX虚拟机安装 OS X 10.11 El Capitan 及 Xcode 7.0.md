---
layout: post
title:  "在 Win7-Win8下使用 VirtualBOX虚拟机安装 OS X 10.11 El Capitan 及 Xcode 7.0"
date: 2016-07-30 21:46:08 +0800
tags: 
slug: p20160730214608
---

# 在 Win7-Win8下使用 VirtualBOX虚拟机安装 OS X 10.11 El Capitan 及 Xcode 7.0





## 在 Win 7或8 下使用 VirtualBOX 虚拟机安装 OS X 10.11 El Capitan 及 [Xcode](https://so.csdn.net/so/search?q=Xcode&spm=1001.2101.3001.7020) 7.0


来源：<http://bbs.feng.com/read-htm-tid-9908410.html>   ( OS X and Xcode 7.0 请参考本链接)  
 <http://www.wikigain.com/install-mac-os-x-el-capitan-virtualbox/> (泪推！！还是要信Google，得永生啊！！![抓狂](http://static.blog.csdn.net/xheditor/xheditor_emot/default/crazy.gif)！)  
 


<https://xuanwo.org/2015/08/09/vmware-mac-os-x-intro/>  （参考其“*提示VBoxManage不是可执行的命令*” 部分，及自行搜索：CMD切换操作目录命令）  
 


## 


## 建议电脑要求


    Windows 7或8或8.1, 32或64 bit，在 Win10 下需要使用 Win 8 相容模式启动 VirtualBOX  
     CPU Intel Core i5 / i7，要有VT-x硬件加速  
     内存 4GB 以上  
     硬盘 500GB 以上，NTFS 格式  
   
 由于虚拟机不支持Apple Quartz Extreme/Core Image，需要 Quartz Extreme 的应用软件例如 iBooks Author，Pixelmator，SketchBook 等不能在虚拟机下使用。  
   
 虚拟硬盘文件种子下载: ![](http://bbs.feng.com/static/image/filetype/torrent.gif)OS X 10.11 El Capitan GM Candidate.torrent*(17.27 KB, 下载次数: 9218)*    
       或 网盘下载![](http://images.weiphone.net/data/attachment/forum/201510/02/121705yce2zfooq9uoqe5a.gif)(虚拟硬盘文件+下面安装步骤的截图) 链接:<http://pan.baidu.com/s/1jGERDqQ> 密码: hxjm  
 VirtualBox 4.3.18 下载:  <http://pan.baidu.com/s/1kTiv0wf>  
       或  [http://www.virtualbox.org/](http://www.virtualbox.org/wiki/Download_Old_Builds_4_3_pre24)  
   
 安装步骤  
 ⑴ 下载及解压 OS X 10.11 El Capitan GM Candidate by TechReviews.rar  
   
 ⑵ 如尚未安装，下载及双击安装  VirtualBox-4.3.18-96516-Win.exe  
 



![](http://images.weiphone.net/data/attachment/forum/201509/28/021908rwwcaabqoapjr5m8.png)

  

  

⑶.1 在 VirtualBOX 新建虚拟电脑 
  
 名称        :   
OSXElCapitan
  
 类型        :   
Mac OS X
  
 版本        :   
Mac OS X 10.9 Mavericks (64 bit)  
  如果没有64bit的选项，请Google: “How to enable CPU Hardware Virtualize in BIOS Setup”
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/031742rqu2yq1deq1quw55.png)

  

  

⑶.2 内存分配最少 2048 MB 以上 
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/022249oc2qwocw5h2oz2iv.png)

  

  

⑶.3 点选 
使用已有的虚拟硬盘文件, 选择种子下载及解压后的文件 
OS X 10.11 El Capitan GM by TechReviews.vmdk 后点击 
创建按钮 
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/022507gsqla6qu8m2bju9q.png)

  

  

⑶.4 打开 VirtualBOX 虚拟机的 
设置
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/030331knzqwkuuhwgwv75r.png)

  

  
 检查及修改 
系统主板, 去掉 "软驱"，硬盘选择上移，芯片组改为 
 PIIX3，设置如下图 
  
 系统 -> 处理器，如果你的机器是4核,可选择双核 CPU 数量 = 2 
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/031802ayzs89tmdq9id8wm.png)

  

  

⑶.5 显示 -> 显存大小设置到最大 
 128 MB
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/031811j00umel0jrlfwjfa.png)

  

  

⑷.1
关闭 VirtualBOX 
  

  

⑷.2 鼠标右键单击 并选择 
以系统管理员身份再执行 VirtualBOX 
  

![](http://images.weiphone.net/data/attachment/forum/201410/23/043112kt13pdaktttk866t.png)
  

  

⑷.3 在 Windows 搜索 cmd , 鼠标右键单击选择 
以系统管理员身份执行 cmd 命令行 
  

![](http://images.weiphone.net/data/attachment/forum/201410/23/050615jagmofkilb7foouo.png)
  

  

⑷.4 请根据 ⑶.1 虚拟电脑的名称 
 OSXElCapitan，在 cmd  命令行依次输入命令如下： 
  
 （ 
<https://xuanwo.org/2015/08/09/vmware-mac-os-x-intro/>  （参考其“ 
*提示VBoxManage不是可执行的命令*” 部分，及自行搜索：CMD切换操作目录命令）） 


1. cd "C:\Program Files\Oracle\VirtualBox"  （这里是你的安装目录，如果不是默认路径，请改至相应路径）
2. VBoxManage.exe modifyvm "OSXElCapitan"  --cpuidset 00000001 000306a9 04100800 7fbae3ff bfebfbff
3. VBoxManage setextradata "OSXElCapitan"  "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "MacBookPro11,3"
4. VBoxManage setextradata "OSXElCapitan"  "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"
5. VBoxManage setextradata "OSXElCapitan"  "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Iloveapple"
6. VBoxManage setextradata "OSXElCapitan"  "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"
7. VBoxManage setextradata "OSXElCapitan"  "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 1




```
    cd "C:\Program Files\Oracle\VirtualBox"
    VBoxManage.exe modifyvm "OSXElCapitan"  --cpuidset 00000001 000306a9 04100800 7fbae3ff bfebfbff
    VBoxManage setextradata "OSXElCapitan"  "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "MacBookPro11,3"
    VBoxManage setextradata "OSXElCapitan"  "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"
    VBoxManage setextradata "OSXElCapitan"  "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Iloveapple"
    VBoxManage setextradata "OSXElCapitan"  "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"
    VBoxManage setextradata "OSXElCapitan"  "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 1
```

  
 修改屏幕分辨率： 
   


```
    VBoxManage setextradata "OSXElCapitan"  "VBoxInternal2/EfiGopMode" 4
```
其中数字表示如下, 虚拟机默认分辨率值是 2 – 1024×768 
   
 0 – 640×480 
   
 1 – 800×600 
   
 2 – 1024×768 
   
 3 – 1280×1024 
   
 4 – 1440×900 
   







---


  

  

⑸.1 关闭 VirtualBOX 及 关闭 cmd 命令行窗口 
  

  

⑸.2 重新正常启动 VirtualBOX, 及 
启动 OSXElCapitan 虚拟机文件 
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/031724h581ltmnjtjm5ys3.png)

  

  

⑸.3 进入 Mac OS X 后，开始设置国家 
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/032426q7zvw770zww17vyr.png)

  

  

⑸.4 如果没有 Apple ID 可选择不登录 
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/032441u69q62n1ixkw67g7.png)

  

  

⑸.5 输入用户名称和用户初始密码 
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/032456epem167yiiwe564p.png)

  

  

⑸.6 设置时区 
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/032506a0cayqs3uvefqksx.png)

  

  

⑸.7 完成其他安装步骤后，并成功进入 Mac OS X 系统，开始系统偏好设置, 更改语言 (左上角的 苹果菜单 -> 系统偏好设置(System Preferences) -> Language & Text, 
重新启动后才会更新
  


![](http://images.weiphone.net/data/attachment/forum/201509/28/032517zsxt5lf5ys5k5cet.png)

  

  

⑸.8 重新启动，开始使用Mac OS X 10.11 El Capitan


==========================================


### 【已解决】关于Apple ID登录App Store停住的问题


http://www.wikigain.com/install-mac-os-x-el-capitan-virtualbox/  
 这里介绍了同样的安装OS X 10.11 EI Capitan的方法，但是它下面的回复贴出了AppleID登录问题的可能的解决方案：  
 Edwin says:  
 July 21, 2016 at 9:57 PM  
 Solution:  
   
 The following procedure worked for me. Please note that it will delete your Apple ID account information for the current user and you will need to log in again.  
 if the App Store is opened and stuck in the login process, force close it  
 open the Terminal  
 issue the following commands:  
 sudo pkill -9 -f Account （这里是要kill掉app store, You can also use Activity Monitor）  
 sudo rm $HOME/Library/Accounts/\*  
 open the App Store and log in  
   
 https://discussions.apple.com/message/29192197#29192197


  
 


===========================================



Xcode 下载: 


链接: <http://pan.baidu.com/s/14YF0M>密码: zcew



Xcode 7.1 beta 2 + Command Line Tools (10.11 El Capitan):  
 ![](https://img-blog.csdn.net/20160731182328323?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


  
 


  
 Xcode 7.0.1 + Command Line Tools (10.11 El Capitan) + Command Line Tools (10.10 Yosemite):  
 ![](https://img-blog.csdn.net/20160731182430671?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


  
 



其他安装，或需要更改安全性和隐私，屏幕分辨率，请参考 <http://bbs.feng.com/read-htm-tid-8474315.html>  
 本地机器连接到虚拟机方案 请参考 ⑻.2   <http://bbs.feng.com/read-htm-tid-7625465.html>  
   
 





---



  
 **相关帖子**  
   
 iOS 8 开发的好东西【网盘下载】(新增 iOS 9 开发的好东西) <http://bbs.feng.com/read-htm-tid-8721978.html>  
 在 Win 7或8 下使用 VirtualBOX 虚拟机安装 OS X 10.10 Yosemite 及 Xcode 6.1 <http://bbs.feng.com/read-htm-tid-8474315.html>  
 安装 THEOS, Xcode 6.1, 及 升级 MacOS X 10.9.4 的方法, 请参考  [http://bbs.feng.com/read-htm-tid-7625465.html](http://bbs.feng.com/forum.php?mod=viewthread&tid=7625465&page=1#pid111803260)  
 在 Win 7 下使用 VirtualBOX 虚拟机安装 OS X 10.9 Mavericks 及 Xcode 5 <http://bbs.feng.com/read-htm-tid-7625465.html>  
 在 Win 7 下使用 VirtualBOX 虚拟机安装 OS X 10.8 Mountain Lion 及 Xcode 4.5 <http://bbs.feng.com/read-htm-tid-5329046.html>


  

  


  

  





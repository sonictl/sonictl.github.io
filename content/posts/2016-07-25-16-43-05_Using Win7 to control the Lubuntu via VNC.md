---
layout: post
title:  "Using Win7 to control the Lubuntu via VNC"
date: 2016-07-25 16:43:05 +0800
tags: 
slug: p20160725164305
---

# Using Win7 to control the Lubuntu via VNC





 有时候在嵌入式计算机没有显示器键鼠供其使用，但又需要通过Terminal或者图形界面对它进行操作，可通过远程连接软件进行。比如vnc 
  

## Using Win7 to control the Lubuntu via [VNC](https://so.csdn.net/so/search?q=VNC&spm=1001.2101.3001.7020)


### ===== Lubuntu VNC Server installation =====


#### 1. install tools: tightvncserver autocutsel



```
sudo apt-get install tightvncserver autocutsel
```
 #tightvncserver is vnc server application 
  
 #autocutsel is for getting bidirectional clipboard sharing 
  

  

#### 2. setup password to access desktops: orangepi


  

#### 3. Run the server once and kill it:

    Commands: 
  

  


```
   $ tightvncserver :1
   $ tightvncserver -kill :1

```

  

#### 4. modify the ~/.vnc/xstartup file:

    commands: 
  


```
   $ vim ~/.vnc/xstartup
```

  

   file content:




```
#!/bin/sh

xrdb $HOME/.Xresources
xsetroot -solid grey
#x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
#x-window-manager &
# Fix to make GNOME work
export XKL_XMODMAP_DISABLE=1
/etc/X11/Xsession
```

  
 change the above into: 
  

 ----------------------- 
  


```
#!/bin/sh

#xrdb $HOME/.Xresources
xsetroot -solid grey
#x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
#x-window-manager &
# Fix to make GNOME work
export XKL_XMODMAP_DISABLE=1
#/etc/X11/Xsession
autocutsel -fork
openbox &
lxterminal &
/usr/bin/lxsession -s Lubuntu -e LXDE &
```

-----------------------


#### 5. Start the vnc server as normal:


   Commands:



```
   $ su
   $ tightvncserver :1
```

  
 Now you're ready to use the other machine(Windows/Linux/Mac) to remote control this machine: <ip\_address>:1 
  

  
 refer: 
<http://www.2cto.com/os/201305/213718.html>  (Chines) 
  

  

#### 6. For instance: Using Win7 to control the Lubuntu via vnc:

    在windows用VncViewer登录桌面 
  
    下载VncViewer: http://www.realvnc.com/download/viewer/ 
  
   
  
    打开程序后按如下格式输入VNC Server地址： VNCServer IP地址:会话号     
  
   （如，我的ubuntu ip为192.168.4.108，回话号为5，则输入 192.168.4.108:5） 
  
   
  
 连接，  然后输入前面设置的密码。OK，现在远程成功了。 
  

  

  
 ---------- 
  
 reference: 
  
 Chinese: 
<http://www.cnblogs.com/lanxuezaipiao/p/3724958.html>
  
 Chinese: 
<http://www.2cto.com/os/201305/213718.html>
  
 English:  
<https://ubuntuforums.org/showthread.php?t=2193100>
  
 English:  
<http://vandorp.biz/2012/01/installing-a-lightweight-lxdevnc-desktop-environment-on-your-ubuntudebian-vps/>
  
 English:  
<https://wiki.ubuntu.com/Lubuntu/RemoteDesktop>
  

  

  

### ======= Lubuntu Setup ========


<https://help.ubuntu.com/community/Lubuntu/Setup>
  
 This officail web contains the items below: 
  
      Configuration 
  
         Workarounds and Guides 
  
         Packages 
  
         Fonts 
  
             Change Font Size in Interface 
  
             Install and view fonts in Lubuntu 
  
         Boot, Install, Login, Logoff 
  
             Add LXDE to Ubuntu 
  
         Find Version of Lubuntu 
  
         Windows, Desktop, Taskbar, LXDE 
  
         Files 
  
         Keyboard, Mouse, Trackpad 
  
         Time and Language 
  
         Networking 
  
         Sound 
  
             Sound Preferences 
  
             Playing Audio CDs 
  
             Testing Sounds with Sound Recorder 
  
             Change Default Sound card 
  
             Checking Pulseaudio 
  
         Video and Graphics 
  
     Installed Tools 
  
         System Tools 
  
         Preferences 
  
     Applications 
  
         Graphics 
  
         Sound & Video 
  
         Internet 
  
         Office 
  
         Accessories 
  
         Games 
  
     Topics 
  
         Fixing Your Third Party App 
  
         Coming from Windows 
  
             Running Windows Programs 
  
             Defragment 
  
             Antivirus 
  
             Firewall 
  
             Clean System 
  
     Maintenance 
  
         Get Updated Software 
  
         Check Hardware 
  
         Lost Password 
  
         Share Files and Folders 
  
     Customizing 
  
         Edit the Menu 
  
         Change Look and Appearance 
  

  




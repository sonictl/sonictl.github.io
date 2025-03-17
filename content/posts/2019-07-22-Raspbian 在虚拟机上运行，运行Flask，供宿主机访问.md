---
layout: post
title: 'Raspbian 在虚拟机上运行，运行Flask，供宿主机访问'
date: 2019-07-22 08:25:00 +0800
category: from_cnblogs
slug: p20190722082500
---
# Raspbian 在虚拟机上运行，启动Flask，供宿主机访问

1. 参考ref 1, 在virtualbox上跑起来Raspbian OS
2. 参考ref 2, 在Raspbian上安装并运行Falsk, 注意要外网访问得有个参数 --host=0.0.0.0
3. 参考ref 3, 用端口映射方法将内网ip暴露给宿主机。 映射主机 192.168.56.5556 到 宿主机 0.0.0.0:5000

在主机浏览器访问：192.168.56.5556 即可看到下图：
![](https://img2018.cnblogs.com/blog/780771/201907/780771-20190722162432706-173335492.png)



**reference: **
  http://www.penguintutor.com/raspberrypi/rpi-desktop-virtualbox
  https://flask.palletsprojects.com/en/1.0.x/quickstart/
  https://www.cnblogs.com/Reyzal/p/7743747.html
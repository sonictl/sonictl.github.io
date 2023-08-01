---
layout: post
title: '【未显示首页】Access IOT (Arduino) from anywhere remotely'
date: 2020-12-18 15:45:00 +0800
category: from_cnblogs
slug: p20201218154500
---
How to Access Arduino Video Stream Over Internet

I'm working on an Arduino project that capture video and stream the video via web service.
The ESP32-CAM moduel is ready-to-use and it provide a web service in LAN (e.g. access it via 192.168.1.11).

I hope to access this video streaming web service from Internet. I'm familiar with the process of accessing local network service via LAN exposing services like FRP or Ngrok. I hope to find some frp-like open source code that is compatible with Arduino.

There are actually two ways in which we could connect our arduino to the internet (as far as I know).

Method 1) Making our arduino as a web server, using which we can send commands and control the things that are connected to the arduino. Also, you can store and access files remotely.
ref: https://www.instructables.com/Accessing-Arduino-over-internet/

Method 2) Making our arduino as a client and send the data collected by the arduino to a server which is hosted over the internet. Where you can store and analyse relatively large data.

I'm searching for solutions for Method 2 above.

--------- Solutions --------
The similar but posting images project using PHP is here:
https://randomnerdtutorials.com/esp32-cam-http-post-php-arduino/


ESP8266 FROM ANYWHERE (depends on GL-INET and WEAVED platform https://remote.it)
https://www.instructables.com/ESP8266-FROM-ANYWHERE/


nabto IOT platform looks tailored. But not perfect, cuz it's not free and opensource lib.
https://www.nabto.com/esp32/
https://www.nabto.com/tutorials/


Home Assistant on RaspberryPi, and frpc on RaspberryPi. This is a solution.
https://randomnerdtutorials.com/esp32-cam-video-streaming-web-server-camera-home-assistant/

Router Port Forward
https://www.youtube.com/watch?v=6ZukQCWEDCM
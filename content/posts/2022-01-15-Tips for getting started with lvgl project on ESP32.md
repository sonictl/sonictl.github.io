---
layout: post
title:  "[ESP32_lvgl]Tips for getting started with lvgl project on ESP32"
date: 2022-01-15 14:02:54 +0800
categories: jekyll
slug: p20220115140254
---
# Tips for getting started with lvgl project on ESP32

Before you get started with lvgl on ESP32, the first problem is which SDK(firmware platform) is going to be based on.
You may dev ESP32 project based on Arduino or ESP-IDF(espreessif officially offered).

There are two pathes at least to implement ESP32 project. By the way, the esp32_library for arduino can be used as a submoduel(component) of ESP-IDF SDK.


## path1: ESP-IDF: Develop ESP32 lvgl project with vsCode, platformIO and ESP-IDF
   before start, you need to know how to implement a "Hello world" or "LED blinking" project using the vsCode+platformIO+ESP_IDF


## path2: Arduino: Develop ESP32 lvgl project with vsCode, platformIO and Arduino

  Arduino platform support lvgl not perfectly. You may meet version_conflict problems and obtain tough feeling when compile error exists.



The **path1** is recommended.


======= read more about lvgl =========
https://docs.lvgl.io/latest/en/html/get-started/arduino.html
https://github.com/lvgl/lvgl/tree/master/demos/widgets
https://github.com/lvgl/lv_port_esp32
https://github.com/lvgl/lv_platformio


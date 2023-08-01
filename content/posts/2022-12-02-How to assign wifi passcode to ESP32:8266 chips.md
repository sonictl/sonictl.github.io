---
layout: post
title:  "How to assign wifi passcode to ESP32/8266 chips"
date: 2022-12-02 18:19:54 +0800
categories: IoT Arduino
slug: p20221202181954
---
## Sumerizing: The ways and implementations for assigning wifi passcode to ESP32/ESP8266 MCU 

Assigning WiFi pass code to ESP chip is ubiqutous.

To summerize, we can do it with ways listed below:

- AP mode: A web running on chip, Chip working as Station mode.
- Bluethooth. (ESP8266 has no bluetooth moduel.)
- ESP Touch App with ESP smartConfig© solution. (5G Hz is not supported.)
- others, such as AP-STA link by routers, AP by cellphone, wifi manag-frame broadcasting.

**Comparations with methods above:**

| **Tech Methods** | **UserConfident** | **SuccessRate** | **Popularity**              |
| ---------------- | ----------------- | --------------- | --------------------------- |
| softAP Mode      | middle            | High            | High                        |
| SmartConfig      | High              | Low             | High                        |
| Bluetooth        | High              | High            | middle (esp8266 has no BLE) |

Ps: *ESP32:Update Wi-Fi Credentials Using Bluetooth Application*, [link](https://www.instructables.com/Avoid-Hard-Coding-ESP32-Update-Wi-Fi-Credentials-U/)



#### Method 1: softAP for wifi passcode assigning

Refer: https://sonictl.github.io/iot/arduino/2021/08/26/p20210826204201.html

#### Method 2: smartConfig© with ESPTouch App

This way is cool. **2.4G wifi required.**

Implemention about it can refer:

- [ESP32 Using SmartConfig](https://www.techtonions.com/esp32-using-smartconfig/)

- [SmartConfig: Turn an ESP8266 Into a Smart Home Device](https://www.eeweb.com/smartconfig-how-to-turn-an-esp8266-into-a-smart-home-device/)
- [EsptouchForAndroid](https://github.com/EspressifApp/EsptouchForAndroid)



### To learn: Script for controlling the IoT devices

To control an IoT device, we need to build a script system to represent commands such as SensorParamReading, DoActionOnTime, IF, ForLoop, Judgement, etc.


##### References:

Patent: [终端控制脚本的生成方法和装置](https://patents.google.com/patent/CN107450899A/zh)

https://cs.uwaterloo.ca/~brecht/courses/854-IoT-2016/readings/programming/scriptIoT-iot-journal-2016.pdf

Aliyun IoT platform: [Script syntax](https://www.alibabacloud.com/help/en/iot-platform/latest/script-syntax)





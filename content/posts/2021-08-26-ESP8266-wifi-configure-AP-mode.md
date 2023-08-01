---
layout: post
title:  "ESP8266 wifi configuration with AP mode"
date: 2021-08-26 20:42:01 +0800
categories: IoT Arduino
slug: p20210826204201
---
#ESP8266 wifi configuration with AP mode

Thanks for the github repository: `tzapu/WiFiManager`

Although we all know that we can use the AP mode to configure the wifi password for ESP8266, it still takes time to implement the specific code line by line. Now, with the help of this project, I just need to share the steps to get this project running on our own ESP8266 chip. 

**Note** that the ESP8266 SmartConfig is another covenient way for wifi password broadcasting.

## wifikit8

https://heltec.org/product/wifi-kit-8/

with esp8266 as the MCU on board.

## PlatformIO on VS Code

reference: [Getting start with Platform on VS_Code](https://www.electronicshub.org/programming-esp8266-using-vs-code-and-platformio/)

add `monitor_speed = 115200` into `platformio.ini` file.

Here I have an example code in `main.cpp` for testing:

```c++
#include <Arduino.h>

#define ledPin 4

void setup() {
  // put your setup code here, to run once:
  Serial.begin(115200);
  pinMode(ledPin, OUTPUT);
}

void loop() {
  // put your main code here, to run repeatedly:
  digitalWrite(ledPin, LOW);
  Serial.println("LED is ON!!!");
  delay(1000);

  digitalWrite(ledPin, HIGH);
  Serial.println("LED is OFF!!!");
  delay(1000);
}
```

Once you have successfully implemented your first Arduino proj. with the help of PlatformIO on VSCode, go ahead to read the next section of WiFiManger.




## WiFiManager

### Install the libray in PlatformIO

reference: [install tutorial](https://github.com/tzapu/WiFiManager#install-using-platformio)

in VSCode, goto: PlO_home >> libraries >> search the `tzapu WiFiManger` , and add it to your project.

### Basic exaple code

The basic [example](https://github.com/tzapu/WiFiManager/blob/master/examples/Basic/Basic.ino) helps you get started.



## HTTPS client

reference: [ESP8266 HTTPS client without cert fingerprint](https://buger.dread.cz/simple-esp8266-https-client-without-verification-of-certificate-fingerprint.html)

ESP8266HTTPClient requires server certificate fingerprint hardcoded (or provided somehow) in your sketch to be able to connect. This is not an issue until the certificate on the server is renewed. So here the no-cert https client is used.

#### When avoid this method that without certificate?

Really don't use this when:

- any kind of personal or sensitive data are involved / transfered - like passwords, keys, personal messages
- faked value / information may cause damage - like turning on all heaters in your house during your 2 weeks long summer vacation because somebody faked the API you are using to control them, or bot which sells all your bitcoins by "mistake" because of faked exchange rate received



## Basic simple web server by Python

Python run a web server and publish some messages that can be gotten by the https client on ESP8266.

ref: [https://docs.python.org/3/library/http.server.html](https://docs.python.org/3/library/http.server.html)




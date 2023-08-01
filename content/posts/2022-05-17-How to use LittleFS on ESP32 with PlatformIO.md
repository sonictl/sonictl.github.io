---
layout: post
title:  "How to use LittleFS on ESP32 with PlatformIO"
date: 2022-05-17 11:19:54 +0800
categories: jekyll
slug: p20220517111954
---
# How to use LittleFS on ESP32 with vsCode+platfromIO

As a suplement of this tutorial [How to use LittleFS on ESP8266](https://randomnerdtutorials.com/esp8266-nodemcu-vs-code-platformio-littlefs/) I follows some reference and examples to build this blog.

Get to konw some basic introductions about SPIFFS and [LittleFS](https://www.mischianti.org/2021/04/01/esp32-integrated-littlefs-filesystem-5/#LittleFS_File_System) on ESP8266 and ESP32 is necessary for a noob.

**Note: Most of the functions for SPIFFS operations are compatible for LittleFS.**

follow the tutorials one by one:

	- SPIFFS for ESP8266
	- SPIFFS for ESP32
	- LittleFS for ESP8266
	- LittleFS for ESP32

LittleFS is newly introduced into ESP32 SDK (Aug 2021) . By May2022, the default ESP32-arduino SDK  (arduino-esp32 core) usd by platformIO  is version 1.0.3, which is not supporting LittleFS originally. You'll meet that the `<LittleFS.h>` is missing.

Next, discuss how to upload littleFS files with platform IO.

## 2. ESP32: Build and upload LittleFS filesystem image with platformIO

Ref: [ESP32 integrated littleFS file system](https://www.mischianti.org/2021/04/01/esp32-integrated-littlefs-filesystem-5/)

   Different version of arduino-esp32 core will have different way of using littlefs file system.
   So, in vsCode+platformIO IDE, framework of Arduino, on ESP32, using littlefs is a little more tricky.

we have two ways (two versions of core), you can select:

### way1: Create a ESP32 proj in platformio, rely on default arduino-esp32 core (v1.x.x) ---

in arduino-esp32 core of version 1.0.x, there is no library for littleFS. while[ this repo](https://github.com/lorol/LITTLEFS) offers.

- create a project with `platformio.ini`as below

```
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
monitor_speed = 115200
upload_speed = 230400
board_build.filesystem = littlefs
```

- create `data` folder under root of project

- create /data/test1.txt file with random letters.

- Create /data/subfolder/test2.txt with random letters.

- in platfomio's task list, run **Build Filesystem Image** task. You may see:

  ```
  > Executing task: platformio run --target buildfs --environment esp32dev <
  
  Processing esp32dev (platform: espressif32; board: esp32dev; framework: arduino)
  ...
  ...
  ...
  Building in release mode
  Building FS image from 'data' directory to .pio/build/esp32dev/littlefs.bin
  /subfolder/test2.txt
  /test1.txt
  ```

- in platformio's task list, run **Upload Filesystem Image**

- use the code from [library example](https://github.com/espressif/arduino-esp32/blob/master/libraries/LittleFS/examples/LITTLEFS_PlatformIO/src/main.cpp), build and upload to ESP32 board to test. If you see `subfolder` in the output under `DIR` list. It looks like littleFS is rightly built.

  ```
  Listing directory: /
  Listing directory: /
    FILE: /file1.txt  SIZE: 3  LAST WRITE: 2020-10-06 15:10:33
    DIR : subfolder  LAST WRITE: 2022-05-19 05:33:49
    DIR : /testfolder  LAST WRITE: 2020-10-06 15:10:33
    FILE: test1.txt  SIZE: 55  LAST WRITE: 2022-05-19 05:33:56
  Creating Dir: /mydir
  Creating Dir: /mydir
  Dir created
  Dir created
  Writing file: /mydir/hello2.txt
  Writing file: /mydir/hello2.txt
  - file written
  
  ...
  ```

  

### way2: Using LittleFS for ESP32 with platformio on arduino-esp32 core of version 2.0.3 ---

1. to use the arduino-esp32 core of version 2.0.x in platformIO, add/replace the platformio.ini file with two lines below:

   ```
   platform = https://github.com/platformio/platform-espressif32.git#feature/arduino-upstream
   platform_packages = framework-arduinoespressif32 @ https://github.com/espressif/arduino-esp32.git#2.0.1
   ```

2. create 'data' folder at proj root

3. Create 'subfolder' under 'data' : <proj. root>/data/subfolder, and put random files into data and subfolder like:

   ```
   /data/test.txt
   /data/subfolder/test2.txt
   ```
   
4. Put `mklittlefs` in root of proj.. download that exe for your OS [from a zipped binary here](https://github.com/earlephilhower/mklittlefs/releases).

5. put the [littlefsbuilder.py](https://github.com/espressif/arduino-esp32/blob/master/libraries/LittleFS/examples/LITTLEFS_PlatformIO/littlefsbuilder.py) in proj. root.

   Add to `platformio.ini`: `extra_scripts = littlefsbuilder.py`

   This py makes PlatformIO believe it has created a SPIFFS.

   My littlefsbuilder.py (OSX for mac) :

   ```
   Import("env")
   print(" <littlefsbuilder.py>: make platformIO believes that it crates a SPIFFS filesystem.")
   env.Replace( MKSPIFFSTOOL=env.get("PROJECT_DIR") + '/mklittlefs' )  # PlatformIO now believes it has actually created a SPIFFS
   
   ```

   

6. run **Build filesystem** and **Upload filesystem** in platformIO task list.

    terminal log for build filesystem:

   ```
   Building in release mode
    <littlefsbuilder.py>: make platformIO believes that it crates a SPIFFS filesystem.
   Building SPIFFS image from 'data' directory to .pio/build/esp32dev/spiffs.bin
   /subfolder/test2.txt
   /test.txt
   ```

   

7. For more details, follow this readme file: [README.md](https://github.com/espressif/arduino-esp32/blob/master/libraries/LittleFS/examples/LITTLEFS_PlatformIO/README.md) . and refer README in [this repo](https://github.com/lorol/LITTLEFS) .

8. compare the diff on diff filesystems:
9. ![image-20220519175936884](/assets/images/image-20220519175936884.png)





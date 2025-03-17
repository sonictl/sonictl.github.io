---
layout: post
title:  "[ESP32_lvgl]Use LVGL with Micropython for ESP32 devBoard"
date: 2022-01-19 22:15:45 +0800
categories: 
 - embedded
 - esp32
 - lvgl
slug: p20220119221545
---

# [ESP32_lvgl]Use LVGL with Micropython for ESP32 devBoard

## Table of contents:

[TOC]

---

**Table of contents:**

[1 Some basic concepts](#1-some-basic-concepts)

[2 The one: lv_micropython](#2-the-one-lvmicropython)

[3 Quick use of lv_micropython](#3-quick-use-of-lvmicropython)

   [3.1  Build micropython for Linux port (optional for newbie):](#31--build-micropython-for-linux-port-optional-for-newbie)

   [3.2 Building `lv_micropython` micropython fork for ESP32 port:](#32-building-lvmicropython-micropython-fork-for-esp32-port)
   
     [3.2.1 install the ESP-IDF sdk](#321-install-the-esp-idf-sdk)
     
        [Get ESP-IDF v4.2](#get-esp-idf-v42)
        
        [Set up the environment variables](#set-up-the-environment-variables)
        
     [3.2.2 Build the `lv_micropython` firmware](#322-build-the-lvmicropython-firmware)
     
     [3.2.3 Flashing the `lv_micropython` firmware](#323-flashing-the-lvmicropython-firmware)
     
     [3.2.4 Getting a Python prompt on the device](#324-getting-a-python-prompt-on-the-device)

[4 Confiture wifi and use the board](#4-confiture-wifi-and-use-the-board)
[Reference and read more](#reference-and-read-more)

---
First, thanks for this blog post: [Micropython + LittlevGL](https://blog.lvgl.io/2019-02-20/micropython-bindings) written by amirgon on Feb, 2019.

Micropython Binding for LittlevGL ([`lv_binding_micropython`](https://github.com/lvgl/lv_binding_micropython)) was designed to make it simple to use LittlevGL with Micropython. It plays as a bridge between micropython and lvgl libraries.

In principle `lv_binding_micropython` can support any micropython fork. 

To add `lv_binding_micropython` to some Micropython fork you need to add `lv_binding_micropython` under Micropython lib as a git submodule.

**Note:** In this blog, all the implementations, commands and configurations were tested in Ubuntu20.04 virtual machine.


### 1 Some basic concepts

**Before continue, we need to know some concepts:**

 *what is a micropython fork?*
 > a fork is like a unix distribution. or a micropython that complied/built from fork-and-modified code. "There are many micropython forks for a variety of systems and hardware platforms not supported in the mainline." -- [wikipedia_page](https://en.wikipedia.org/wiki/MicroPython). 

 *What is a git submodule?*
 > 
 > In Git you can add a submodule to a repository. This is basically "a repository embedded in your main repository". A git submodule is a record within a host git repository that points to a specific commit in another external repository.

 *How to add some repository under Micropython lib as a git submodule?*

 > 
 > You can refer to [`Managing Python dependencies with git submodules`](https://kif11.github.io/2016/02/13/Managing-Python-dependencies-with-git-submodules.html) and the commands they used shows you some concepts about submodule:
 > `git submodule add {github_repo} {local_path}`
 > 
 > Example:
 >
 > `git submodule add https://github.com/Kif11/logger modules/logger`
 > 
 >   or:
 > 
 > `git submodule add https://github.com/lvgl/lvgl libs/lvgl`
 > 
 > To understand the submodule, you can see this picture:
 > 
 > ![image-20220118220911083](/assets/images/image-20220118220911083.png)



*what is micropython binding module?*

> In order to have access to C libraries from Python, a dynamic-link library can be written in C in such a way that `CPython` will treat it as a module (you will be able to import it in your Python programs). The Python/C API allows the library to define functions that are written in C but still callable from Python. The API is very powerful and provides functions to manipulate all Python data types and access the internals of the interpreter.
>
> I didn't find accureate definitions about micropython binding module, however, readers can guess that the binding module should be a C/python bridge library.
>
> For more info, refer: https://stackoverflow.com/questions/10202306/python-bindings-how-does-it-work

*what is a micropython port?* 

> A port will typically contain definitions for multiple “boards”, each of which is a specific piece of hardware that that port can run on, e.g. a development kit or device.



So, 

**Before compile the lvgl_contained micropython, you need to do:**

 - add some lines to Micropython Makefile in order to create LittlevGL Binding module
 - add the new lvgl module to Micropython by editing mpconfigport.h, in order to compile LittlevGL

ref: [MicroPython external C modules](https://docs.micropython.org/en/v1.17/develop/cmodules.html?highlight=mpconfigport#micropython-external-c-modules)


----
### 2 The one: lv_micropython

As an example, [Amirgon](https://blog.lvgl.io/2019-02-20/micropython-bindings) has created the `lv_micropython` - a Micropython fork with LittlevGL binding. `lv_micropython` usually be used directly by developers. Let's see how to use it for ESP32 board. Can use it as it is, or as an example of how to integrate LittlevGL with Micropython. 

[`lv_micropython`](https://github.com/littlevgl/lv_micropython) can currently be used with **LittlevGL** on the **unix port** and on the **ESP32 port**.

LittlevGL needs drivers for the display and for the input device. The Micropython binding contains some example drivers that are registered and used on [`lv_micropython`](https://github.com/littlevgl/lv_micropython):

- SDL unix drivers (display and mouse)
- ILI9341 driver for ESP32.
- Raw Resistive Touch for ESP32 (ADC connected to screen directly, no touch IC)

For details about the `lv_micropython`: https://github.com/lvgl/lv_micropython

### 3 Quick use of lv_micropython

First, testing the building for Unix(Linux) port:

#### 3.1  Build micropython for Linux port (optional for newbie):

Here, I tested all the commands below on a virtual machine running Ubuntu 20.04.

> sudo apt-get install build-essential libreadline-dev libffi-dev git pkg-config libsdl2-2.0-0 libsdl2-dev python3.7 parallel

251MB will be occupied.

> cd ~/Downloads
> 
> git clone https://github.com/lvgl/lv_micropython.git
> 
> cd lv_micropython
> 
> git submodule update --init --recursive lib/lv_bindings
> 

waiting for the updating of submodules

> make -C mpy-cross

it shows:
```
make: Entering directory '/home/user/Downloads/lv_micropython/mpy-cross'
Use make V=1 or set BUILD_VERBOSE in your environment to increase build verbosity.
LVGL-GEN build/lvgl/lv_mpy.c
cp ../lib/lv_bindings/driver/png/lodepng/lodepng.cpp build/lodepng/lodepng.c
LODEPNG-GEN build/lodepng/mp_lodepng.c
mkdir -p build/genhdr
GEN build/genhdr/mpversion.h
GEN build/genhdr/moduledefs.h
...
mkdir -p build/py/
mkdir -p build/shared/runtime/
CC ../py/mpstate.c
CC ../py/nlr.c
CC ../py/nlrx86.c
CC ../py/nlrx64.c
CC ../py/nlrthumb.c
...
CC main.c
CC gccollect.c
CC ../shared/runtime/gchelper_generic.c
LINK mpy-cross
   text	   data	    bss	    dec	    hex	filename
 410368	  37952	    896	 449216	  6dac0	mpy-cross
make: Leaving directory '/home/user/Downloads/lv_micropython/mpy-cross'

```

> make -C ports/unix submodules

It shows:
```
make: Entering directory '/home/user/Downloads/lv_micropython/ports/unix'
Use make V=1 or set BUILD_VERBOSE in your environment to increase build verbosity.
Updating submodules: lib/axtls lib/berkeley-db-1.xx lib/libffi
Submodule 'lib/axtls' (https://github.com/micropython/axtls.git) registered for path '../../lib/axtls'
Submodule 'lib/berkeley-db-1.xx' (https://github.com/pfalcon/berkeley-db-1.xx) registered for path '../../lib/berkeley-db-1.xx'
Submodule 'lib/libffi' (https://github.com/atgreen/libffi) registered for path '../../lib/libffi'
Cloning into '/home/user/Downloads/lv_micropython/lib/axtls'...
Cloning into '/home/user/Downloads/lv_micropython/lib/berkeley-db-1.xx'...
Cloning into '/home/user/Downloads/lv_micropython/lib/libffi'...
Submodule path '../../lib/axtls': checked out '531cab9c278c947d268bd4c94ecab9153a961b43'
Submodule path '../../lib/berkeley-db-1.xx': checked out '35aaec4418ad78628a3b935885dd189d41ce779b'
Submodule path '../../lib/libffi': checked out 'e9de7e35f2339598b16cbb375f9992643ed81209'
make: Leaving directory '/home/user/Downloads/lv_micropython/ports/unix'

```

> make -C ports/unix

It shows:
```
make: Entering directory '/home/user/Downloads/lv_micropython/ports/unix'
Use make V=1 or set BUILD_VERBOSE in your environment to increase build verbosity.
LVGL-GEN build-standard/lvgl/lv_mpy.c
cp ../../lib/lv_bindings/driver/png/lodepng/lodepng.cpp build-standard/lodepng/lodepng.c
LODEPNG-GEN build-standard/lodepng/mp_lodepng.c
mkdir -p build-standard/genhdr
GEN build-standard/genhdr/mpversion.h
GEN build-standard/genhdr/moduledefs.h
...
mkdir -p build-standard/build-standard/lvgl/
...
CC ../../lib/lv_bindings/driver/SDL/modSDL.c
CC ../../lib/lv_bindings/driver/linux/modfb.c
LINK micropython
   text	   data	    bss	    dec	    hex	filename
1452759	 206648	   6568	1665975	 196bb7	micropython
make: Leaving directory '/home/user/Downloads/lv_micropython/ports/unix'

```

You can run the build result by:
> ./ports/unix/micropython

For further testing, comment out the last three lines for `/lib/lv_bindings/examples/advanced_demo.py`, and run the demo by the command:
> ./ports/unix/micropython ./lib/lv_bindings/examples/advanced_demo.py

You'll see a window shows up:

![image-20220119174750811](/assets/images/image-20220119174750811.png)

Congrates!

#### 3.2 Building `lv_micropython` micropython fork for ESP32 port:

For building a port of MicroPython to the Espressif ESP32 series of microcontrollers. It uses the ESP-IDF framework. The MicroPython runs as a task under FreeRTOS.

Reference: https://github.com/lvgl/lv_micropython/blob/master/ports/esp32/README.md#setting-up-the-toolchain-and-esp-idf

MicroPython on ESP32 requires the Espressif IDF version 4 (IoT development framework, aka SDK). The ESP-IDF includes the libraries and RTOS needed to manage the ESP32 microcontroller, as well as a way to manage the required build environment and toolchains needed to build the firmware.


##### 3.2.1 install the ESP-IDF sdk

To install the ESP-IDF the full instructions can be found at the Espressif Getting Started guide: [installation step by step](https://docs.espressif.com/projects/esp-idf/en/release-v4.2/esp32/get-started/index.html#installation-step-by-step).

For my vm ubuntu, I follow the installing prerequests tools with next command:
`sudo apt-get install git wget flex bison gperf python3 python3-pip python3-setuptools cmake ninja-build ccache libffi-dev libssl-dev dfu-util`

Adding user to dialout on Linux
 > sudo usermod -a -G dialout $USER

###### Get ESP-IDF v4.2

 > mkdir -p ~/esp
 > 
 > cd ~/esp
 > 
 > git clone -b release/v4.2 --recursive https://github.com/espressif/esp-idf.git

 Set up the tools for esp32

 > cd ~/esp/esp-idf
 > 
 > ./install.sh esp32


###### Set up the environment variables
 need to add tools to the PATH environment variable. ESP-IDF provides `export.sh` script which does that.

 > . $HOME/esp/esp-idf/export.sh

 add the below line into `~/.bashrc`, then restart terminal.
 `alias get_idf='. $HOME/esp/esp-idf/export.sh'`

##### 3.2.2 Build the `lv_micropython` firmware
 cd to the root of this repository:
 `cd $HOME/Downloads/lv_micropython`
 make the mpy-cross

 > make -C mpy-cross

 Then to build MicroPython for the ESP32 run:

 >  cd ports/esp32
 > 
 >  make submodules

you'll see:

```
git submodule update --init ../../lib/berkeley-db-1.xx
```

 >  make

or, for ILI9341 screen driver compatible:

 ```
 cd $HOME/Downloads/lv_micropython
 make -C ports/esp32 LV_CFLAGS="-DLV_COLOR_DEPTH=16 -DLV_COLOR_16_SWAP=1" BOARD=GENERIC
 make -C ports/esp32 deploy
 ```

it shows:
```
idf.py -D MICROPY_BOARD=GENERIC -B build-GENERIC  build
Executing action: all (aliases: build)
Running cmake in directory /home/user/Downloads/lv_micropython/ports/esp32/build-GENERIC
Executing "cmake -G Ninja -DPYTHON_DEPS_CHECKED=1 -DESP_PLAT
...
MPY webrepl_setup.py
MPY websocket_helper.py
MPY neopixel.py
GEN /home/user/Downloads/lv_micropython/ports/esp32/build-GENERIC/frozen_content.c
[1447/1447] Generating binary image from built executable
esptool.py v3.1-dev
Merged 2 ELF sections
Generated /home/user/Downloads/lv_micropython/ports/esp32/build-GENERIC/micropython.bin

Project build complete. To flash, run this command:
/home/user/.espressif/python_env/idf4.2_py3.8_env/bin/python ../../../../esp/esp-idf/components/esptool_py/esptool/esptool.py -p (PORT) -b 460800 --before default_reset --after hard_reset --chip esp32  write_flash --flash_mode dio --flash_size detect --flash_freq 40m 0x1000 build-GENERIC/bootloader/bootloader.bin 0x8000 build-GENERIC/partition_table/partition-table.bin 0x10000 build-GENERIC/micropython.bin
or run 'idf.py -p (PORT) flash'
bootloader  @0x001000    22576  (    6096 remaining)
partitions  @0x008000     3072  (    1024 remaining)
application @0x010000  2258256  (  101040 remaining)
total                  2323792

```

This will produce a combined `firmware.bin` image in the `build-GENERIC/` subdirectory (this firmware image is made up of: bootloader.bin, partitions.bin and micropython.bin).

However I obtains two `.bin` files:

```
$ ll build-GENERIC/*.bin
-rw-rw-r-- 1 xxx 2319696 1月  19 20:53 firmware.bin
-rw-rw-r-- 1 xxx 2258256 1月  19 20:53 micropython.bin

```
##### 3.2.3 Flashing the `lv_micropython` firmware

on ubuntu, you may need to use this command to add permission for $USER to access the usbSerial port:
`sudo adduser $USER dialout`
This will add the current user to the dialout group. Login and out it to take effect.

Erase the flash completely:
```
$ cd $HOME/Downloads/lv_micropython/ports/esp32
$ get_idf
$ make erase

```
use `picocom -b 115200 /dev/ttyUSB0` to test the communication if you meet "connection time out" error when erasing.
after erasing, you'll see:

```
Serial port /dev/ttyUSB0
Connecting........_
Chip is ESP32-D0WDQ6 (revision 1)
Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
Crystal is 40MHz
MAC: 4c:eb:d6:7d:0b:44
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 460800
Changed.
Erasing flash (this may take a while)...
Chip erase completed successfully in 9.2s
Hard resetting via RTS pin...
Done

```

**Tips:** You may meet the "time out connecting fatal" when using `make erase` in ubuntu guest vm. I finally use the ESP-IDF vsCode extension to successfully erase the flash of my ESP32 DevKit_v1_board. 

To flash the MicroPython firmware to your ESP32 use:

`$ make deploy`

you'll see:
```
idf.py -D MICROPY_BOARD=GENERIC -B build-GENERIC  -p /dev/ttyUSB0 -b 460800 flash
Executing action: flash
Running ninja in directory /home/user/Downloads/lv_micropython/ports/esp32/build-GENERIC
Executing "ninja flash"...
[2/5] Performing build step for 'bootloader'
ninja: no work to do.
[2/3] cd /home/user/esp/esp-idf/components/esptool_py && /usr/bin/cm...C" -P /home/user/esp/esp-idf/components/esptool_py/run_esptool.cmake
esptool.py esp32 -p /dev/ttyUSB0 -b 460800 --before=default_reset --after=hard_reset write_flash --flash_mode dio --flash_freq 40m --flash_size 4MB 0x8000 partition_table/partition-table.bin 0x1000 bootloader/bootloader.bin 0x10000 micropython.bin
esptool.py v3.1-dev
Serial port /dev/ttyUSB0
Connecting....
Chip is ESP32-D0WDQ6 (revision 1)
Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
Crystal is 40MHz
MAC: 4c:eb:d6:7d:0b:44
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 460800
Changed.
Configuring flash size...
Flash will be erased from 0x00008000 to 0x00008fff...
Flash will be erased from 0x00001000 to 0x00006fff...
Flash will be erased from 0x00010000 to 0x00237fff...
Compressed 3072 bytes to 117...
Writing at 0x00008000... (100 %)
Wrote 3072 bytes (117 compressed) at 0x00008000 in 0.1 seconds (effective 476.7 kbit/s)...
Hash of data verified.
Compressed 22576 bytes to 14067...
Writing at 0x00001000... (100 %)
Wrote 22576 bytes (14067 compressed) at 0x00001000 in 0.8 seconds (effective 221.6 kbit/s)...
Hash of data verified.
Compressed 2258256 bytes to 1423672...
Writing at 0x00010000... (1 %)
Writing at 0x000194cc... (2 %)
Writing at 0x000210ff... (3 %)
...
Writing at 0x00231716... (100 %)
Wrote 2258256 bytes (1423672 compressed) at 0x00010000 in 52.4 seconds (effective 344.7 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
Done

```

Note: the above "make" commands are thin wrappers (like and alias) for the underlying `idf.py` build tool that is part of the ESP-IDF. You can instead use idf.py directly.

##### 3.2.4 Getting a Python prompt on the device
You can get a prompt via the serial port, via `/dev/ttyUART0`, which is the same UART that is used for programming the firmware. The baudrate for the REPL is 115200 and you can use a tools such as:
```
$ picocom -b 115200 /dev/ttyUSB0
```
or

```
$ miniterm.py /dev/ttyUSB0 115200
```
Tips:

- `ctrl-A, ctrl-X` will terminate `picocom` session.

- You can also use `idf.py monitor`.

After get python prompt, type `help()` shows the Control Commands.

![image-20220119220949837](/assets/images/image-20220119220949837.png)

### 4 Confiture wifi and use the board
go to link [configure wifi and use the board](https://github.com/lvgl/lv_micropython/blob/master/ports/esp32/README.md#configuring-the-wifi-and-using-the-board)



### Reference and read more

- Reference: https://github.com/lvgl/lv_micropython/blob/master/ports/esp32/README.md

- more info about micropython lvgl development: https://github.com/lvgl/lv_micropython

- more info about replanting on ARM board: [支持LVGL的micropython固件编译（二）arm板移植](https://blog.csdn.net/qq_34440409/article/details/118421452)  (in Chinese)



----

## Issue fixing after flashing

I met `RuntimeError: ili9341 micropython driver requires defining LV_COLOR_DEPTH=16` when using REPL with my esp32_TFTscreen board driven by ILI9341.
Add the next configure into command for firmware building. Refer: https://github.com/lvgl/lv_micropython/README.md

 > LV_CFLAGS="-DLV_COLOR_DEPTH=16 -DLV_COLOR_16_SWAP=1" 

Refer `3.2.2 Build the lv_micropython firmware`

you may need to delete the build-XXX folder under `lv_micropython/ports/esp32/` before re-build the `firmware.bin`

```
>>> from ili9XXX import ili9341
>>> disp = ili9341()
Single buffer
ILI9341 initialization completed
Enable backlight

```

When you see the above lines in REPL, the `LV_COLOR_DEPTH=16` parameter for ILI9341 is successfully added.

### Finally giving up

Micropython developing lvgl powered UI for ESP32 seems a joke.

I spent one hour for running an example micropython lvgl code, but failed. Nothing shown on TFT screen.

This experience of building / deploying micropython tells me that micropython is a tool, if large lib such as lvgl is depended, your micropython project will be tough.

c++ is still the right way to play with embedded projects. micropython is a tool for simple control and simple logic.


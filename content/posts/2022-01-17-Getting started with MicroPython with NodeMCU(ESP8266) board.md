---
layout: post
title:  "Getting started with MicroPython with NodeMCU(ESP8266) board"
date: 2022-01-17 12:55:38 +0800
categories: jekyll
slug: p20220117125538 
---
# Getting started with MicroPython with NodeMCU(ESP8266) board


## 1. Follow the official tutorial to flash the MicroPython firmware

Link: 

- https://docs.micropython.org/en/latest/esp8266/quickref.html
- https://docs.micropython.org/en/latest/esp8266/tutorial/intro.html#intro

Comands you may need:

```
$ esptool.py --port /dev/tty.usbserial-14120 erase_flash
$ esptool.py --port /dev/tty.usbserial-14120 --baud 460800 write_flash --flash_size=detect -fm dout 0  esp8266-20210902-v1.17.bin
```

you'll receive message from ESP8266 on serial monitor such as puTTy:

```
microPython v1.17 on 2021-09-02; ESP module with ESP8266
Type "help()" for more information.
```

micropython script can be run in the REPL shell.

The Adafruit `ampy` tool gives you more function and recommended to use it. https://learn.adafruit.com/micropython-basics-load-files-and-run-code

Go to use vsCode to develop micropython project after you tested the MCU's REPL in puTTy.



## 2. vsCode + microPython_vsCode_extension for micropython development

Create a new project using this extension. In vscode, cmd+shift+P and enter micropythonIDE, you'll see the Run, Stop, and Getting started functions. 

![image-20220117120039545](/assets/images/image-20220117120039545.png)

edit the main.py

<span style="color:red">Stop the code running before flashing the code into board. </span> Use this extension to stop/start a code running on board.



## 3. vsCode + Pymakr + micropy-cli for micropython project development

**Warning:** On my Mac OSX, the Pymakr is not working stably. You may meet terminal not launching problem. Instead, the micropython vsCode extension is recommended on OSX. The Pymakr cannot list the usbSerial port (CH340 chip) on my osx. 

Next notes are following this blog post: https://lemariva.com/blog/2018/12/micropython-visual-studio-code-as-ide

#### 2.1 install the Pymakr vscode extension 

Pymakr requires node.js installed.

After installation, the Pymakr Console will open, and you should have new commands at the bottom bar. TERMINAL will show you:

 ```
Welcome to the Pymakr plugin! Use the buttons on the left bottom to access all features and commands.
This is how you get started:
 1: Open 'Global Settings' (we went ahead and did that for you)
 2: Connect a pycom board to your USB and the terminal will auto-connect to it (you can skip step 3 and 4 now)
 3: If you want to connect over WiFi, disable auto_connect and fill in the correct IP or serial port of your Pycom board in 'address'
     (When using serial, you can also use the 'List serial ports' to find the correct serial port)
 4: Connect using the 'Connect' command or the 'Pymakr Console' button
 5: Open a micropython project with main.py and boot.py files
 6: Start running files and uploading your code 

 ```


Configure it with  `Ctrl+Shift+P` and search for `Pymakr > Global Settings` for all projects, or `Pymakr > Project Settings` for one project.

For more info, refer: https://randomnerdtutorials.com/micropython-esp32-esp8266-vs-code-pymakr/#connecting

After the configuration, test by connecting the board with dev-PC machine.

Create a folder, open it with vsCode, and create the two files below in it.

- **boot.py**: runs before user's code, it sets up configuration and initializations;
- **main.py**: the main script that contains your code. It is executed immediately after the *boot.py*.

Upload the code onto board: Click the `upload` button after the main.py edited and saved.

**Important note:** 

- To make further changes (uploading a new code), press **CTRL**+**C** to stop any running code. You won’t be able to upload new code if the ESP is running another code. 

- You also won’t be able to execute any other commands while the board is running a code. If the prompt (>>>) is not displayed, it means the board is running a code. 



#### fix the pyMakr extension bug:

follow the link: https://lemariva.com/blog/2018/12/micropython-visual-studio-code-as-ide#vscode-pymakr-error-update-01082019-560219

The problem of *usbSerial port not listed* is still not solved.



#### 2.3 install the  `micropy-cli` Python module to get more powerful utilities

more powerful utilities such as intellisense, auto-complete, etc can be offered by `micropy-cli` module.

Follow this blog post: https://lemariva.com/blog/2019/08/micropython-vsc-ide-intellisense

Activate the microPython virtual env that contains `python3.x` and `pip`.

install the module with `pip install --upgrade micropy-cli`

After installing the `micropy-cli` module, you can open VSCode and then, open the folder in which you have your project. Then, using the Terminal that you got on VSCode (if you don't see it, hit on `Ctrl+\`` (Ctrl + back-tick character), or click the '+' button on the right, or drag the lower horizontal bar up) and type the following to search and install a MicroPython stub (ESP8266 here).

```
micropy stubs search esp8266
```

use cmd `micropy stubs add esp8266-micropython-1.11.0` to install the latest stub for ESP8266

 follow the link: https://lemariva.com/blog/2019/08/micropython-vsc-ide-intellisense#installing-micropy-cli-398769







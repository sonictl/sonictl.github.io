---
layout: post
title:  "Complile the lvgl sample proj. in visual studio Code (vsCode) with help of ESP-IDF extension"
date: 2022-01-11 21:09:54 +0800
categories: jekyll
slug: p20220111210954
---


# Complile the lvgl sample project in visual studio Code (vsCode) with help of ESP-IDF extension

If you've go through esp-idf vscode extension installation, esp-idf blink example, just skip the **section 1** and **section 2**. Directly check the lvgl example project `lv_port_esp32` cloning/configure/build/etc. steps in **section 3**.

## Implement a "Hello world" or "LED blinking" project using the vsCode+platformIO+ESP_IDF

**Prerequisit:** Go through the official "get started with ESP-IDF" tutorial (**highly recomemded**, [link](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html))

### section 1. install the esp-idf vscode extension

  Note: if you have go through the official "get started with ESP-IDF" tutorial (**highly recomemded**, [link](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html)), you can specify the path of ESP-IDF code and tools when install the ESP-IDF vscode extension. If your machine/PC has no ESP-IDF downloaded, just keep the default options when installing the ESP-IDF vscode extension

  tip for esp-idf tool path: 
   [Customizing the tools installation path](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html#customizing-the-tools-installation-path)
   default path for IDF_TOOLS_PATH: $HOME/.espressif/tools


  After ESP-IDF vscode extension is installed and configured, it supports for all actions related to ESP-IDF namely build, flash, monitor, debug, tracing, core-dump, System Trace Viewer, etc.

  Review the [tutorials](https://github.com/espressif/vscode-esp-idf-extension/blob/master/docs/tutorial/toc.md) for ESP-IDF Visual Studio Code Extension to learn how to use all features.

### section 2. follow the tutorial for first "hello world" proj.
  Follow [this tutorial](https://github.com/espressif/vscode-esp-idf-extension/blob/master/docs/tutorial/basic_use.md)
  in the vscode window, open the panel with shortcuts: cmd+shift+P (mac) or click vscode‘s menu `View` -> `Command Palette`
  enter ESP-IDF, the panel will show the options about ESP_IDF.

  e.g.  go to panel: ESP-IDF: Show Examples Projects 

  more steps:
    You can debug ESP-IDF projects as shown in the [debug tutorial](https://github.com/espressif/vscode-esp-idf-extension/blob/master/docs/tutorial/debugging.md).

### section 3. clone a third-party code repository and configure/build/etc with the ESP_IDF_vscode_ext.

  Take the [lv_port_esp32](https://github.com/lvgl/lv_port_esp32) as an example:
```bash
  git clone --recurse-submodules https://github.com/lvgl/lv_port_esp32.git

  cd ~//vsCodePlatformIO/udrDev_proj/espIDF_vscode_lvgl_port_esp32
```
  open vscode, cmd+shift+P (mac), ESP-IDF: import ESP-IDF proj.

  import the folder of that proj repository.

  cmd+shift+P (mac), ESP-IDF: SDK configuration editor(menuconfig)
  configure the project. and the `sdkconfig` file will be updated each time the configuration saved.

  Build, flash, etc. using the bottom bar of vscode UI, given by ESP-IDF_vscode_extension.

  *You may meet difficulties in searching for the right configurations for your ESP32 dev board.* Rightly choose the VSPI/HSPI, TFT controller, etc. is nontrivial. Ask the board offerer if needs.


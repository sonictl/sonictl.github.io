---
layout: post
title:  "[ESP32_lvgl]Complile the lvgl sample proj. in visual studio Code (vsCode) with help of ESP-IDF extension"
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
  open vscode, **cmd+shift+P** (mac), ESP-IDF: import ESP-IDF proj.

  import the folder of that proj repository. You may need to open that folder and directly edit it. The import function seems not working.

  **cmd+shift+P** (mac), ESP-IDF: SDK configuration editor(menuconfig)

  configure the project. and the `sdkconfig` file will be updated each time the configuration saved.

  Build, flash, etc. using the bottom bar of vscode UI, given by ESP-IDF_vscode_extension.

**Note:**  **You may meet difficulties in searching for the right configurations** for your ESP32 dev board. Rightly choose the VSPI/HSPI, TFT controller, etc. is nontrivial. Ask the board offerer if needs.

Here, I offer one, which is for my board with ILI9341 and XPT2046:

```
# === some configuration parameters modified for my board ====

# lv_examples configuration
# CONFIG_LV_DEMO_WIDGETS_SLIDESHOW is not set

# LVGL configuration
CONFIG_LV_HOR_RES_MAX=320
CONFIG_LV_VER_RES_MAX=240

# Theme usage (can only check one)
CONFIG_LV_THEME_EMPTY=y
CONFIG_LV_THEME_TEMPLATE=y
CONFIG_LV_THEME_MONO=y

# CONFIG_LV_THEME_DEFAULT_FLAG_LIGHT is not set (the LIGHT is optional)
CONFIG_LV_THEME_DEFAULT_FLAG_DARK=y


# CONFIG_DISPLAY_ORIENTATION_PORTRAIT is not set (set depends on your situation)
CONFIG_DISPLAY_ORIENTATION_LANDSCAPE_INVERTED=y
CONFIG_LV_DISPLAY_ORIENTATION=3

# CONFIG_LV_PREDEFINED_PINS_NONE is not set
CONFIG_LV_PREDEFINED_PINS_30=y

#
# Display Pin Assignments
#
CONFIG_LV_ENABLE_BACKLIGHT_CONTROL=y
CONFIG_LV_BACKLIGHT_ACTIVE_LVL=y
CONFIG_LV_DISP_PIN_BCKL=33

#
# LVGL Touch controller
#
CONFIG_LV_TOUCH_CONTROLLER=1
# CONFIG_LV_TOUCH_CONTROLLER_NONE is not set
CONFIG_LV_TOUCH_CONTROLLER_XPT2046=y
CONFIG_LV_TOUCH_DRIVER_PROTOCOL_SPI=y
CONFIG_LV_TOUCH_CONTROLLER_SPI_HSPI=y
# CONFIG_LV_TOUCH_CONTROLLER_SPI_VSPI is not set
#
# Touchpanel (XPT2046) Pin Assignments
#
CONFIG_LV_TOUCH_SPI_MISO=12
CONFIG_LV_TOUCH_SPI_MOSI=13
CONFIG_LV_TOUCH_SPI_CLK=14
CONFIG_LV_TOUCH_SPI_CS=27
CONFIG_LV_TOUCH_PIN_IRQ=25
# end of Touchpanel (XPT2046) Pin Assignments
#
# Touchpanel Configuration (XPT2046)
#
CONFIG_LV_TOUCH_X_MIN=200
CONFIG_LV_TOUCH_Y_MIN=120
CONFIG_LV_TOUCH_X_MAX=1900
CONFIG_LV_TOUCH_Y_MAX=1900
# CONFIG_LV_TOUCH_XY_SWAP is not set
# CONFIG_LV_TOUCH_INVERT_X is not set
CONFIG_LV_TOUCH_INVERT_Y=y
CONFIG_LV_TOUCH_DETECT_IRQ=y
# CONFIG_LV_TOUCH_DETECT_IRQ_PRESSURE is not set
# CONFIG_LV_TOUCH_DETECT_PRESSURE is not set
# end of Touchpanel Configuration (XPT2046)

CONFIG_IPC_TASK_STACK_SIZE=1024
CONFIG_MB_TIMER_PORT_ENABLED=y





```

in **sdkconfig GUI given by ESP-IDF**, you can configure below as same effects by above parameters:

```
#####################################
# go to esp-idf extension GUI for sdkconfig

path: Component config > lv_examples configuration
  unCheck: Slide demo widgets automatically.
  
path: component config > LVGL configuration
  Maximal horizontal resolution to support by the library = 320
  Maximal vertical resolution to support by the library   = 240

path: component config > LVGL configuration > theme usage > Enable theme usage, always enable at least one theme
  Check all themes
  Select theme default flag : dark

path: Component config > LVGL TFT Display controller > Display orientation
  LANDSCAPE_INVERTED

path: Component config > LVGL TFT Display controller > Select predefined board pinouts
  ESP32 Devkit v1 with 30 pins

path: Component config > LVGL TFT Display controller > Display Pin Assignments
  check: Enable control of the display backlight by using an GPIO.
  check: Is backlight turn on with a HIGH (1) logic level?
  GPIO for Backlight Control = 33


path: Component config > LVGL Touch controller > Select a touch panel controller model.
  select: XPT2046
  select: Touch Controller SPI Bus. = HSPI
    
configure XPT2046 Pin Assignments:
  CONFIG_LV_TOUCH_SPI_MISO=12
  CONFIG_LV_TOUCH_SPI_MOSI=13
  CONFIG_LV_TOUCH_SPI_CLK=14
  CONFIG_LV_TOUCH_SPI_CS=27
  CONFIG_LV_TOUCH_PIN_IRQ=25

path: Component config > LVGL Touch controller > Touchpanel Configuration (XPT2046)
  uncheck: Swap XY.
  uncheck: Invert X coordinate value.
  check: Invert Y coordinate value.
  Select touch detection method. = IRQ pin only

path: Component config > IPC (Inter-Processor Call) > Inter-Processor Call (IPC) task stack size
  1024

path: Component config > Modbus configuration
  check: Modbus stack use timer for 3.5T symbol time measurement
         If this option is set the Modbus stack uses timer for T3.5 time measurement. Else the internal UART TOUT timeout is used for 3.5T symbol time measurement.

# Some notes:
keep default configure display Pin Assignments:
  Component config
  LVGL TFT Display controller
  Display Pin Assignments
  GPIO for MOSI (Master Out Slave In)
  13

  GPIO for MISO (Master In Slave Out)
  GPIO for CLK (SCK / Serial Clock)
  14

  Use CS signal to control the display
  GPIO for CS (Slave Select)
  15

  Use DC signal to control the display
  GPIO for DC (Data / Command)
  2
  GPIO for Reset
  4

  Enable control of the display backlight by using an GPIO.

  Is backlight turn on with a HIGH (1) logic level?
  GPIO for Backlight Control
  33
```

About configuration, read more about [pre-configuration for some boards.](https://github.com/lvgl/lv_port_esp32/blob/master/PRECONFIGURED_KITS.md)

---

**Later on, for testing, I did some fresh installing and `lv_port_esp32` initiallization for development other project on Ubuntu and Win11.**

Some notes for steps:

#### Ubuntu 20.04: fresh install vscode and esp-idf extension, start new development from lv_port_esp32

**Steps**:

   \1. install the esp-idf vscode extension.

   \2. git clone recursively with components for the repo: lv_port_esp32

   \3. open the repo lv_port_esp32 in vsCode

   \4. esp-idf: configure sdk for this proj (refer above parameters, or, copy the sdkconfig file from fine-tuned proj that works well on your board)

   \5. build, flash and test the demo given by lv_port_esp32

   \6. modify the code below:

​        6.1: add `#include "../components/lv_examples/lv_examples/lv_examples.h"`

​        6.2: in `create_demo_application` function, comment out `lv_demo_widgets();` 

​        6.3: add `lv_ex_get_started_1();` at the end of `create_demo_application` function.

   \7. build, falsh and test the modified proj.

Tips: in ubuntu, development configurations are smooth. While in microsoft Win11, not smooth with bash, terminal, and esp-idf.

#### Windows11: Fresh install vscode and esp-idf extension, start new development from lv_port_esp32

I try to freshly start a esp-idf lvgl proj i.e. `lv_port_esp32` on windows11, but failed
Steps below:

-  install vsCode

-  install esp-idf vscode extension
    ... meet the error: "restart esp-idf extension to launch the wizard" ...

  install git

  in git_bash, cd to the workspace for esp-idf projects

- git clone --recurse-submodules https://github.com/lvgl/lv_port_esp32.git

  ...cloning into <workspace>/lv_port_esp32... (cloning needs proxy)

The error: "restart esp-idf extension to launch the wizard" will not disappear after restarting vsCode over and over again.




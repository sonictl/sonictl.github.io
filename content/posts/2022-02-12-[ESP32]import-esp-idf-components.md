---
layout: post
title:  "[ESP32]import esp-idf components"
date: 2022-02-12 21:39:44 +0800
categories: esp32 embedded
slug: p20220212213944
---

import esp-idf components:

> 1. import the components is easy: git clone , mkdir <proj_dir>/components, and copy into <proj_dir>/components
>
>    The git repos and commands that I used for this testing:
>
>    ```
>    git clone https://github.com/natanaeljr/esp32-I2Cbus.git I2Cbus
>    git clone https://github.com/natanaeljr/esp32-MPU-driver.git MPUdriver
>    mkdir -p <proj_path>/components
>    mv I2Cbus <proj_path>/components
>    mv MPUdriver <proj_path>/components
>    ```
>
>    for more, refer: https://github.com/natanaeljr/esp32-MPU-driver
>
> 2. Do `sdkconfig` for the right chip and protocal for the imported component
>
> 3. if the `hello_world_main.c` is renamed, rename it in `<proj_dir>/main/CMakeLists.txt`




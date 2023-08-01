---
layout: post
title:  "[ESP32_lvgl] From demo to own UI component with lvgl on ESP32"
date: 2022-01-22 17:44:07 +0800
categories: embeded
slug: p20220122174407
---
# [ESP32_lvgl] From demo to own UI component with lvgl on ESP32
This is the FIRST blog for denoting my learning process of lvgl on platform of ESP-IDF for chip ESP32.

After a lot of experinment, I gues ESP-IDF should be the best sdk that suit for ESP32 lvgl development. Rather than the toy -- micropython.

## 1. Motivation and value of this post
After configuring the ESP32 LVGL development environment, developer will try to run demo on the development board. But for beginners, from running demo to developing the specific project, they may meet some little but not-easy-to-solve difficalties. They need to be familiar with the code structure of demo.

This article is the path from demo to developing your own LVGL ESP32 project. So you can really start your own project after reading this blog post.

## 2. Basic introduction and environment
I use the **vsCode** and **ESP-IDF vscode extension** for all the development work. 
Refered tutorial is: 
\[ESP32_lvgl\]Complile the lvgl sample proj. in visual studio Code (vsCode) with help of ESP-IDF extension. [this link](https://sonictl.github.io/jekyll/2022/01/11/p20220111210954.html)

After successfully run the demo on your board, try to use the code below:
 - find 100+ simple examples here: [https://docs.lvgl.io/master/examples.html](https://docs.lvgl.io/master/examples.html)
 - They are from here: [https://github.com/lvgl/lvgl/tree/master/examples](https://github.com/lvgl/lvgl/tree/master/examples)

## 3. What? the example code has no main() function?
Let's discuss the [one menu exmple's code](https://docs.lvgl.io/master/examples.html). In the link above, you can find the 'one menu' example as below:

```c++
#include "../lv_examples.h"
#if LV_BUILD_EXAMPLES && LV_USE_BTN

static void btn_event_cb(lv_event_t * e)
{
    lv_event_code_t code = lv_event_get_code(e);
    lv_obj_t * btn = lv_event_get_target(e);
    if(code == LV_EVENT_CLICKED) {
        static uint8_t cnt = 0;
        cnt++;

        /*Get the first child of the button which is the label and change its text*/
        lv_obj_t * label = lv_obj_get_child(btn, 0);
        lv_label_set_text_fmt(label, "Button: %d", cnt);
    }
}

/**
 * Create a button with a label and react on click event.
 */
void lv_example_get_started_1(void)
{
    lv_obj_t * btn = lv_btn_create(lv_scr_act());     /*Add a button the current screen*/
    lv_obj_set_pos(btn, 10, 10);      /*Set its position*/
    lv_obj_set_size(btn, 120, 50);    /*Set its size*/
    lv_obj_add_event_cb(btn, btn_event_cb, LV_EVENT_ALL, NULL);           /*Assign a callback to the button*/

    lv_obj_t * label = lv_label_create(btn);  /*Add a label to the button*/
    lv_label_set_text(label, "Button");       /*Set the labels text*/
    lv_obj_center(label);
}

#endif
```



If you put the code into `main.c`, it will not compile.

See the `main.c`  code in `lv_port_ESP32/main/main.c` [here_link](https://github.com/lvgl/lv_port_esp32/blob/master/main/main.c)

Try to read and understand the code line by line with the tips in [this quick start page](https://docs.lvgl.io/latest/en/html/get-started/quick-overview.html#quick-overview)

I copy it here:

>## Add LVGL into your project
>
>The following steps show how to setup LVGL on an embedded system with a display and a touchpad.
>
>- [Download](https://github.com/lvgl/lvgl/archive/master.zip) or Clone the library from GitHub with `git clone https://github.com/lvgl/lvgl.git`
>- Copy the `lvgl` folder into your project
>- Copy `lvgl/lv_conf_template.h` as `lv_conf.h` next to the `lvgl` folder, change the first `#if 0` to `1` to enable the file's content and set at least `LV_HOR_RES_MAX`, `LV_VER_RES_MAX` and `LV_COLOR_DEPTH` defines.
>- Include `lvgl/lvgl.h` where you need to use LVGL related functions.
>- Call `lv_tick_inc(x)` every `x` milliseconds **in a Timer or Task** (`x` should be between 1 and 10). It is required for the internal timing of LVGL. Alternatively, configure `LV_TICK_CUSTOM` (see `lv_conf.h`) so that LVGL can retrieve the current time directly.
>- Call `lv_init()`
>- Create a display buffer for LVGL. LVGL will render the graphics here first, and seed the rendered image to the display. The buffer size can be set freely but 1/10 screen size is a good starting point.
>
>```
>static lv_disp_buf_t disp_buf;
>static lv_color_t buf[LV_HOR_RES_MAX * LV_VER_RES_MAX / 10];                     /*Declare a buffer for 1/10 screen size*/
>lv_disp_buf_init(&disp_buf, buf, NULL, LV_HOR_RES_MAX * LV_VER_RES_MAX / 10);    /*Initialize the display buffer*/
>```
>
>- Implement and register a function which can **copy the rendered image** to an area of your display:
>
>```
>lv_disp_drv_t disp_drv;               /*Descriptor of a display driver*/
>lv_disp_drv_init(&disp_drv);          /*Basic initialization*/
>disp_drv.flush_cb = my_disp_flush;    /*Set your driver function*/
>disp_drv.buffer = &disp_buf;          /*Assign the buffer to the display*/
>lv_disp_drv_register(&disp_drv);      /*Finally register the driver*/
>
>void my_disp_flush(lv_disp_drv_t * disp, const lv_area_t * area, lv_color_t * color_p)
>{
>    int32_t x, y;
>    for(y = area->y1; y <= area->y2; y++) {
>        for(x = area->x1; x <= area->x2; x++) {
>            set_pixel(x, y, *color_p);  /* Put a pixel to the display.*/
>            color_p++;
>        }
>    }
>
>    lv_disp_flush_ready(disp);         /* Indicate you are ready with the flushing*/
>}
>```
>
>- Implement and register a function which can **read an input device**. E.g. for a touch pad:
>
>```
>lv_indev_drv_t indev_drv;                  /*Descriptor of a input device driver*/
>lv_indev_drv_init(&indev_drv);             /*Basic initialization*/
>indev_drv.type = LV_INDEV_TYPE_POINTER;    /*Touch pad is a pointer-like device*/
>indev_drv.read_cb = my_touchpad_read;      /*Set your driver function*/
>lv_indev_drv_register(&indev_drv);         /*Finally register the driver*/
>
>bool my_touchpad_read(lv_indev_t * indev, lv_indev_data_t * data)
>{
>    data->state = touchpad_is_pressed() ? LV_INDEV_STATE_PR : LV_INDEV_STATE_REL;
>    if(data->state == LV_INDEV_STATE_PR) touchpad_get_xy(&data->point.x, &data->point.y);
>
>    return false; /*Return `false` because we are not buffering and no more data to read*/
>}
>```
>
>- Call `lv_task_handler()` periodically every few milliseconds in the main `while(1)` loop, in Timer interrupt or in an Operation system task. It will redraw the screen if required, handle input devices etc.



The `create_demo_application()` function in `lv_port_ESP32/main/main.c` is managing the GUI component. Modify it using the code of the 'one menu' example like below:

```c++
static void create_demo_application(void)
{
//     /* When using a monochrome display we only show "Hello World" centered on the
//      * screen */
// #if defined CONFIG_LV_TFT_DISPLAY_MONOCHROME || defined CONFIG_LV_TFT_DISPLAY_CONTROLLER_ST7735S

    /* use a pretty small demo for monochrome displays */
    /* Get the current screen  */
    lv_obj_t * scr = lv_disp_get_scr_act(NULL);

    /*Create a Label on the currently active screen*/
    lv_obj_t * label1 =  lv_label_create(scr, NULL);

    /*Modify the Label's text*/
    lv_label_set_text(label1, "Hello\nWorld");
    lv_obj_align(label1, NULL, LV_ALIGN_CENTER, 0, 0);  // Align the label to center, NULL-align on parent component(screen), 0,0 - x,y offset

    /* The second component, a counting Buttom*/
    lv_obj_t * btn = lv_btn_create(lv_scr_act(), NULL);     /*Add a button the current screen*/
    lv_obj_set_pos(btn, 10, 50);                            /*Set its position*/
    lv_obj_set_size(btn, 300, 50);                          /*Set its size*/
    lv_obj_set_event_cb(btn, btn_event_cb);                 /*Assign a callback to the button*/
    lv_obj_t * label = lv_label_create(btn, NULL);          /*Add a label to the button*/
    lv_label_set_text(label, "Lab of artificial intelligence");                     /*Set the labels text*/


// #else
//     /* Otherwise we show the selected demo */

//     #if defined CONFIG_LV_USE_DEMO_WIDGETS
//         lv_demo_widgets();
//     #elif defined CONFIG_LV_USE_DEMO_KEYPAD_AND_ENCODER
//         lv_demo_keypad_encoder();
//     #elif defined CONFIG_LV_USE_DEMO_BENCHMARK
//         lv_demo_benchmark();
//     #elif defined CONFIG_LV_USE_DEMO_STRESS
//         lv_demo_stress();
//     #else
//         #error "No demo application selected."
//     #endif
// #endif
    // lv_demo_widgets();
}

```

You can get what you want on your TFT screen. Like below:

![image-20220127223244264](/assets/images/image-20220127223244264.png)



## The embarrassing situation of embedded development

Different board, different cpu, different driver chip etc. cause the variety of situation for code configuration.

No `main.c` that works for all board. This result in that the example code is code patch for highly-customized  `main.c`. There is no `main.c` that can be given as an example.

It's difficult for the maintainer to give a `main.c` that contains example code and compilable for each board.

The existance of `lv_port_esp32` is good news for ESP32_lvgl development. Starting from modifying this proj. is a good start for ESP32 board with touch screen.











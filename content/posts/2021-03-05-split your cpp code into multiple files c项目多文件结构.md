---
layout: post
title: 'split your cpp code into multiple files'
date: 2024-07-30 09:22:00 +0800
category: from_cnblogs
slug: p20210305145900
---
# How to split main.c into .h and .c files

> This post is updated on July 29, 2024, with cotent as below:

For a C project which has directory as below:

```
├── CMakeLists.txt
├── README.md
└── main
    ├── CMakeLists.txt
    └── main.c
```

How to split the `main.c` into `me.h` and `me.c` and let all the declarations and definitions be in the .`h` and `.c` file respectively?

### 1. Understand the process of compilation and linking

#### 1.1 Pre-processing 预处理

在编译开始之前，预处理器会处理代码中的预处理指令，比如 `#include`、`#define` 和条件编译指令 (`#ifdef`、`#endif` 等)。

预处理的主要任务是：

- **包含头文件**：预处理器会查找 `#include` 指令指定的头文件，并**将这些文件的内容插入**到包含指令的位置。例如，`#include "me.h"` 会把 `me.h` 文件的内容插入到 `main.c` 文件中。
- **宏展开**：所有的宏定义（使用 `#define`）会被展开到其实际的值。
- **条件编译**：根据条件编译指令（如 `#ifdef`、`#if`）决定是否编译特定的代码部分。

#### 1.2 Complilation 编译

在预处理之后，编译器会将每个源文件（.c 文件）编译成目标文件（.o 文件）。编译的过程包括：语法分析、生成中间代码、优化、生成目标代码（.o 文件，尚未链接）。其中，从中间代码到目标代码，是由assembly汇编步骤完成的。汇编的过程包括：将中间代码转换为汇编语言代码；将汇编语言代码转换为机器码，存储在目标文件中。

#### 1.3 Linking 链接

在编译阶段完成后，所有生成的目标文件（.o 文件）和库文件（.a 或 .lib 文件）会被链接器（Linker）链接在一起，生成最终的可执行文件（.elf、.exe 等）。链接的过程包括：

- **符号解析**：链接器会解析目标文件中的符号（函数名、变量名等），并将这些符号与其他目标文件或库中的符号对应起来。
- **重定位**：链接器会调整目标文件中的地址，使它们指向正确的位置。
- **生成可执行文件**：将所有目标文件和库文件链接在一起，生成最终的可执行文件。

### 2. Understanding Build System

有必要理解构建系统 Build System 的相关概念：

构建系统（如 CMake、Makefile）负责管理整个构建过程，它会自动执行以下任务：

- **查找文件**：根据项目的配置（如 CMakeLists.txt），查找所有需要编译的源文件和头文件。
- **生成构建规则**：根据项目的配置文件生成编译、汇编和链接的规则。
- **执行构建**：按照生成的规则执行编译、汇编和链接过程，生成最终的可执行文件或库文件。

### 3 . How will they handle on this example proj.
以开场时项目结构为例，假设使用 CMake 构建系统：

**预处理阶段**：

- `main.c` 中插入 `#include "me.h"` ，该 `#include` 被处理，`me.h` 的内容被插入到 `main.c` 中。
- `me.c` 中插入 `#include "me.h"` ，该 `#include` 被处理，`me.h` 的内容被插入到 `me.c` 中。

**编译汇编阶段**：

- `main.c` 和 `me.c` 被编译成目标文件（如 `main.o` 和 `me.o`）。

**链接阶段**：

- 链接器将 `main.o` 和 `me.o` 以及其他必要的库文件链接在一起，生成最终的可执行文件（例如 `your_project.elf`）。

**构建系统作用**：

- CMake 根据 `CMakeLists.txt` 文件生成编译和链接规则，然后调用编译器和链接器执行这些规则，最终生成可执行文件。

### 4. 实操

`main.c` 源文件内容如下：

```c
/* Blink Example */
#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/gpio.h"
#include "esp_log.h"
#include "led_strip.h"
#include "sdkconfig.h"

/* Macro definitions */
#define BLINK_GPIO 12
#define GPIO_MODE_OUTPUT 1
#define CONFIG_BLINK_PERIOD 1000

// declarations and definitions
static const char *TAG = "example";
static uint8_t s_led_state = 0;

static void blink_led(void)
{
    /* Set the GPIO level according to the state (LOW or HIGH)*/
    gpio_set_level(BLINK_GPIO, s_led_state);
}

static void configure_led(void)
{
    ESP_LOGI(TAG, "Example configured to blink GPIO LED!");
    gpio_reset_pin(BLINK_GPIO);
    /* Set the GPIO as a push/pull output */
    gpio_set_direction(BLINK_GPIO, GPIO_MODE_OUTPUT);
}

void app_main(void)
{
    /* Configure the peripheral according to the LED type */
    configure_led();

    while (1) {
        ESP_LOGI(TAG, "Turning the LED %s!", s_led_state == true ? "ON" : "OFF");
        blink_led();
        /* Toggle the LED state */
        s_led_state = !s_led_state;
        vTaskDelay(CONFIG_BLINK_PERIOD / portTICK_PERIOD_MS);
    }
}
```

将 `main/main.c` 文件重构，将 `#include` 部分和函数声明移到新的 `me.h` 头文件，将函数定义移到新的 `me.c` 源文件，并在 `main.c` 文件中保留 `app_main()` 函数。

#### 4.1 创建 `me.h`

在 `main` 目录下创建一个名为 `me.h` 的头文件，内容如下：

```c
#ifndef ME_H
#define ME_H

#include <stdio.h>
#include "freertos/FreeRTOS.h"
#include "freertos/task.h"
#include "driver/gpio.h"
#include "esp_log.h"
#include "led_strip.h"
#include "sdkconfig.h"

/* Define GPIO and other configuration settings */
#define BLINK_GPIO 12
#define GPIO_MODE_OUTPUT 1
#define CONFIG_BLINK_PERIOD 1000

/* replace `static` into `extern` for globally ref */
extern uint8_t s_led_state;  //static uint8_t s_led_state = 0 (in oirigin main.c)
extern const char *TAG;  // can not define the value with ` = "example" `

/* remove `static` for globally ref */
void blink_led(void);     // static void blink_led(void){} ,,in oirigin main.c  
void configure_led(void); // static void configure_led(void) ,,in oirigin main.c 

#endif /* ME_H */
```

#### 4.2. 创建 `me.c`

在 `main` 目录下创建一个名为 `me.c` 的源文件，内容如下：

```c
#include "me.h"

const char *TAG = "example";
uint8_t s_led_state = 0; // enabled here, but can also be refered by main.c

void blink_led(void)  //static void blink_led(void){} ,,in main.c originally
{
    gpio_set_level(BLINK_GPIO, s_led_state);
}

void configure_led(void) //static void configure_led(void){} ,,in main.c originally
{
    ESP_LOGI(TAG, "Example configured to blink GPIO LED!");
    gpio_reset_pin(BLINK_GPIO);
    gpio_set_direction(BLINK_GPIO, GPIO_MODE_OUTPUT);
}

```

#### 4.3. 修改 `main.c`

更新 `main.c` 文件，只保留 `app_main()` 函数，并包含 `me.h` 文件：

```c

#include "me.h"

void app_main(void)
{

    /* Configure the peripheral according to the LED type */
    configure_led();

    while (1) {
        ESP_LOGI(TAG, "Turning the LED %s!", s_led_state == true ? "ON" : "OFF");
        blink_led();
        /* Toggle the LED state */
        s_led_state = !s_led_state;
        vTaskDelay(CONFIG_BLINK_PERIOD / portTICK_PERIOD_MS);
    }
}
```

#### 4.4. 修改 `CMakeLists.txt`

在 `main` 目录下的 `CMakeLists.txt` 文件中，添加新的 `me.c` 文件：

```cmake
# idf_component_register(
#           SRCS "main.c"
#           INCLUDE_DIRS "."
#)

idf_component_register(
    SRCS "main.c" "me.c"
    INCLUDE_DIRS "."
)
```

### 总结

1. **`me.h`** 文件：包含了头文件、宏定义和函数声明。
2. **`me.c`** 文件：包含了 `blink_led()` 和 `configure_led()` 函数的实现。
3. **`main.c`** 文件：仅包含 `app_main()` 函数，并引用 `me.h` 头文件。
4. **`CMakeLists.txt`** 文件：更新以包括 `me.c` 源文件。

这样，您的代码将更加模块化，易于维护。

### FAQ:

#### FAQ1: 有关 static 关键字、extern 关键字

```
我注意到在 main/blink_example_main.c 文件中的static void blink_led(void)函数名，移动到me.h进行声明以后，‘static'不见了，是什么原因？
```

> ### Quick Answer:
>
> - 在 .h 头文件中，只声明函数，不需要 `static` 关键字。
> - 在源文件中定义函数时，如果希望函数仅在当前源文件中可见，要使用 `static` 关键字；如果函数需要在其他源文件中使用，则不要使用 `static`。
> - `extern` 关键字用于常量变量的全局共享。
>
> ### Normal Answer:
>
> 在 C 语言中，`static` 关键字有不同的作用，具体取决于它的使用上下文。下面是 `static` 关键字的作用和原因：
>
> #### `static` 关键字的作用
>
> 1. **在函数内部**：`static` 关键字用于定义静态局部变量，这些变量在函数调用结束后不会被销毁，其值会被保留到下次调用函数时。
> 2. **在函数外部（文件级别）**：`static` 关键字用于定义静态函数或静态全局变量，这样的函数或变量只能在定义它的源文件内部使用，不能在其他源文件中引用。
>
> #### 为什么在头文件中移除 `static` 关键字
>
> 在头文件中，只需要声明函数，而不需要定义它。声明函数时，通常不使用 `static` 关键字。原因如下：
>
> - **函数声明**：函数声明告诉编译器函数的存在和其接口（返回类型、参数类型等），但不提供函数的实现。声明函数时不需要 `static`，因为函数的定义可能会在其他源文件中找到，且可能会被多个源文件引用。
> - **函数定义**：函数定义提供了函数的具体实现。如果一个函数定义为 `static`，它的作用范围仅限于定义它的源文件，不能在其他源文件中引用。因此，`static` 关键字在函数定义中用来限制函数的链接性，防止外部源文件引用。
>
> #### 具体到本节的情况
>
> 在 `me.h` 头文件中，需要声明 `blink_led()` 和 `configure_led()` 函数，以便*其他源文件可以使用*这些函数。因此，头文件中只包含函数声明，不需要 `static` 关键字。`static` 关键字的作用是将函数的链接限制在源文件内部，这在头文件中是不适用的。
>
> #### 示例
>
> **在头文件 `me.h` 中声明函数：**
>
> ```c
> #ifndef ME_H
> #define ME_H
> 
> #include <stdio.h>
> #include "freertos/FreeRTOS.h"
> #include "freertos/task.h"
> #include "driver/gpio.h"
> #include "esp_log.h"
> #include "led_strip.h"
> #include "sdkconfig.h"
> 
> #define BLINK_GPIO 12
> #define GPIO_MODE_OUTPUT 1
> #define CONFIG_BLINK_PERIOD 1000
> 
> extern uint8_t s_led_state;  // 这里 用extern, 不用 static
> extern const char *TAG;      // 这里 用extern, 不用 static
> 
> void blink_led(void);     // 这里只声明函数，不用 static
> void configure_led(void); // 这里只声明函数，不用 static
> 
> #endif /* ME_H */
> ```
>
> **在源文件 `me.c` 中定义函数：**
>
> ```c
> #include "me.h"
> 
> const char *TAG = "example";
> uint8_t s_led_state = 0;     // 定义和初始化后, main.c 也可使用该值
> 
> void blink_led(void)
> {
>     /* Set the GPIO level according to the state (LOW or HIGH) */
>     gpio_set_level(BLINK_GPIO, s_led_state);
> }
> 
> void configure_led(void)
> {
>     ESP_LOGI(TAG, "Example configured to blink GPIO LED!");
>     gpio_reset_pin(BLINK_GPIO);
>     /* Set the GPIO as a push/pull output */
>     gpio_set_direction(BLINK_GPIO, GPIO_MODE_OUTPUT);
> }
> ```

#### FAQ2: 添加 include 、scr 文件夹，使项目文件更简洁

```
如何添加 include 、scr 文件夹，以使项目文件组织更专业简洁？
```

> 项目中，如果你希望将 `main` 文件夹下的头文件和源文件管理得更有组织，可以在 `main` 文件夹内创建一个 `include` 文件夹来存放头文件，并将源文件（如 `.c` 文件）放在 `src` 文件夹中。你的目录结构可以调整为：
>
> ```css
> .
> ├── CMakeLists.txt
> ├── README.md
> ├── dependencies.lock
> └── main
>  ├── CMakeLists.txt
>     ├── include
>     │   ├── base_callbacks.h
>     │   ├── base_gui.h
>     │   └── base_include.h
>     ├── src
>     │   ├── base_callbacks.c
>     │   ├── base_gui.c
>     │   └── main.c
>     └── idf_component.yml
>    ```
> 
>在这里，`include` 文件夹包含所有的头文件，而 `src` 文件夹包含所有的源文件。你可以根据需要调整头文件和源文件的位置。
> 
>另外，记得在 `main/CMakeLists.txt` 中调整文件路径，以确保编译器可以找到新的 `include` 和 `src` 文件夹。例如：
> 
>```cmake
> idf_component_register(
>  SRCS "src/base_callbacks.c" "src/base_gui.c" "src/main.c"
>     INCLUDE_DIRS "include"
>    )
> ```
> 
>这样设置之后，CMake 构建系统会正确地包含头文件和源文件。



(↑ updated on 2024-07-30 09:22:40)

---




# split your cpp code into multiple files

reference: http://cse230.artifice.cc/lecture/splitting-code.html

## understande the \#include

The compiler will substitute #include <xyz> with the actual contents of the file xyz.

## the header files

the `.h` file is called the header files. People often use the header file as the one included.
Header files usually have the ending .h and source files usually have the ending .cpp

A header file contains only class and function `declarations`.

Note:
  the declaration is a statement that a class exists and has certain properties and methods, or that a function exists; 
  neither the class methods nor the functions will be defined; 
  their implementations will not be provided in the header file.

We also need to prevent repeated-includes because the compiler is not happy when a class or function or variable of the same name is declared twice.



## The frequent `#ifndef XXX_H`, `#define XXX_H` and `#endif`

Because the compiler is not happy when a class or function or variable of the same name is declared twice. Thus, in every header file (named blah.h in this example), we write the following at the top and bottom:

	```
	#ifndef BLAH_H
	#define BLAH_H
	
	...
	
	#endif
	```

This means “if the name BLAH_H is not already defined, then define it.” If a file is included twice, then BLAH_H will be defined (by the first inclusion) so the entire “if—endif” will be skipped (which is the whole file, because the whole file is between the “if” and “endif”). Of course, BLAH_H can be anything; it could be FOO; we usually write FILE_H in the file named file.h so that we don’t reuse names.

## Example

files in structure:
- main.cpp

  - eclipse.h (eclipse.cpp)

    - shape.h

  - rectangle.h (eclipse.cpp)

    - shape.h

------------

- main.cpp:

```c++
#include <iostream>
#include "rectangle.h"
#include "eclipse.h"
using namespace std;

int main()
{
    Rectangle r;
    r.width = 10;
    r.height = 15;
    r.x = 3;
    r.y = 2;

    cout << r.area() << endl;

    Ellipse e;
    e.major_axis = 3;
    e.minor_axis = 5;
    e.x = 14;
    e.y = 68;

    cout << e.area() << endl;

    return 0;
}
```

- rectangle.h
```c++
#ifndef RECTANGLE_H
#define RECTANGLE_H

#include "shape.h"

class Rectangle : public Shape
{
    public:
    double width;
    double height;

    double area();
};

#endif

```
- rectangle.cpp:

  ```c++
  #include "rectangle.h"
  double Rectangle::area()
  {
      return width * height;
  }
  ```


- eclipse.h
```c++
#ifndef ELLIPSE_H
#define ELLIPSE_H

#include "shape.h"

class Ellipse : public Shape
{
    public:
    double major_axis;
    double minor_axis;
 
    double area();
};

#endif

```

- ellipse.cpp:

  ```c++
  #include "ellipse.h"
  double Ellipse::area()
  {
      return 3.1415926 * major_axis * minor_axis;
  }
  ```


- shape.h:

  ```c++
  #ifndef SHAPE_H
  #define SHAPE_H
  
  class Shape
  {
      public:
      double x;
      double y;
  
      virtual double area() = 0;
  };
  
  #endif
  
  ```
  

### another example:

```text
代码的头文件组织。
It cost me much time and a deep night, so it's worthwhile to read it slowly.
===========================================================
    need to test the tutorial: https://cppguide.readthedocs.io/en/latest/cpp/multifile.html
    when-to-use-extern-in-c: https://stackoverflow.com/questions/10422034/when-to-use-extern-in-c
    C语言在头文件定义全局变量的技巧: https://blog.csdn.net/a1598025967/article/details/105876724


    #ifndef HEADER_FILE 
    #define HEADER_FILE
        the entire header file file 
    #endif

    https://edisciplinas.usp.br/pluginfile.php/5453726/mod_resource/content/0/Extern%20Global%20Variable.pdf : 
    # Best way to declare and define global variables

    The clean, reliable way to declare and define global variables is to use a header file to contain
    an extern declaration of the variable.

    The header is included by the one source file that defines the variable and by all the source
    files that reference the variable. For each program, one source file (and only one source file)
    defines the variable. Similarly, one header file (and only one header file) should declare the
    variable. The header file is crucial; it enables cross-checking between independent TUs
    (translation units — think source files) and ensures consistency.
    Although there are other ways of doing it, this method is simple and reliable. It is demonstrated
    by file3.h, file1.c and file2.c:

    ---- file3.h ---- 
    extern int global_variable; /* Declaration of the variable */

    ---- file1.c ---- 
    #include "file3.h" /* Declaration made available here */
    #include "prog1.h" /* Function declarations */
    /* Variable defined here */
    int global_variable = 37; /* Definition checked against declaration */
    int increment(void) { return global_variable++; }

    ----  file2.c ---- 
    #include "file3.h"
    #include "prog1.h"
    #include <stdio.h>
    void use_it(void)
    {
        printf("Global variable: %d\n", global_variable++);
    }

    ===================
    That's the best way to declare and define global variables.
```


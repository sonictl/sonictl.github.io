---
layout: post
title: 'split your cpp code into multiple files'
date: 2021-03-05 14:59:00 +0800
category: from_cnblogs
slug: p20210305145900
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


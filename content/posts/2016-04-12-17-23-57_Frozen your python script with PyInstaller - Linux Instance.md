---
layout: post
title:  "Frozen your python script with PyInstaller - Linux Instance"
date: 2016-04-12 17:23:57 +0800
tags: 
slug: p20160412172357
---

# Frozen your python script with PyInstaller - Linux Instance





## Frozen your python script with [PyInstaller](https://so.csdn.net/so/search?q=PyInstaller&spm=1001.2101.3001.7020) - Linux Instance


In this article, we will describe an example of frozen your python script with PyInstaller in Linux env.


------------------------  
 




### 1. Pre-Requiremets, Linux


* PyInstaller requires the ldd terminal application to discoverthe shared libraries required by each program or shared library.It is typically found in the distribution-packageglibc or libc-bin.
* It also requires the objdump terminal application to extractinformation from object files.This is typically found in the distribution-packagebinutils.
* Install python-pip:  $ sudo apt-get install python-pip


### 2. Install pyInstaller


       $ sudo pip install pyinstaller  
 


### 3. How to use


       Write your python code and save it as "myscript.py" at the path: ~/ws\_pyi/


      $ cd ~/ws\_pyi


      $ pyinstaller --clean --key <KEY> -F myscript.py


      This will create an excutable file myscript at the path: ~/ws\_pyi/dist/


      For more options and details: <https://pythonhosted.org/PyInstaller/#what-pyinstaller-does-and-how-it-does-it>


### 4. Another app you can use to frozen your .py code: Cython


     As the pyInstaller's manual says:


     *Hiding the Source Code:*  
 


* The bundled app does not include any source code.However, PyInstaller bundles compiled Python scripts (.pyc files).These could in principle be decompiled to reveal the logic ofyour code.
* If you want to hide your source code more thoroughly, one possible optionis to compile some of your modules with **Cython**.Using Cython you can convert Python modules into C and compilethe C to machine language.PyInstaller can follow import statements that refer toCython C object modules and bundle them.
* Additionally, Python bytecode can be obfuscated with AES256 by specifyingan encryption key on PyInstaller's command line. Please note that it is stillvery easy to extract the key and get back the original bytecode, but itshould prevent most forms of "casual" tampering.


-------------------------  
 


 For Details, Please Refer pyInstaller's manual: 
<https://pythonhosted.org/PyInstaller/>
  




文章知识点与官方知识档案匹配，可进一步学习相关知识[Python入门技能树](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)[首页](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)[概览](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)422843 人正在系统学习中

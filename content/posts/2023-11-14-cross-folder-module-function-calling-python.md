---
layout: post
title:  "Across folders' module calling in python"
date: 2023-11-14 22:24:06 +0800
categories: python
slug: p20231114222406
---

# Across folders' module calling in python

This topic involves understainding: 

- how Py modules system works

- organizing your proj. files' structure

- using the appropriate import systements

## 1. Proj. Structure

### 1.1 Packages and Modules

❤️A **module** is a Python file containing code and definitions. A **module** is just a Python program that ends with **.py** extension

❤️A **package** is a directory that contains a special `__init__.py` file, making it a Python package. A folder that contains a module becomes a **package**.

see:

```text
|--package_Name/
| |-- module_1.py
| |-- module_2.py
| |-- module_3.py
```

### 1.2 Hierarchy

**Organize** your project into packages and modules based on functionality.

```text
workspace/
|-- myProjName/
|   |-- __init__.py
|   |
|   |-- myFuns_packs/
|   |   |-- __init__.py
|   |   |-- functionsA.py
|   |   |-- funcModuleB.py
|   |
|   |-- myCode/
|       |-- __init__.py
|       |-- main.py
```

The `__init__.py` files can be empty, <mark>but they are required for Python to treat the directories as packages</mark>.

## Method 1: The sys.path

- Python looks for **modules** in directories listed in `sys.path`
  
  You can include necessary directories by manipulate the `sys.path`: 
  
  ```python
  # python code for main.py
  import sys
  
  sys.path.insert(0, 'path/to/your/module/folder')  # it's enough to run this line once
  # or
  sys.path.insert(0, '../myFuncs_packs')  # it's enough to run this line once
  ```

```
Be cautious about circular imports, where module A imports module B, and module B imports module A. This can lead to unexpected behavior.



## **Method 2: Using the PYTHONPATH** **environment variable**

Similarly, if you don’t want to use the [**sys** module](https://www.geeksforgeeks.org/python-sys-module/) to set the path of the new directory. You can assign a directory path to the [PYTHONPATH](https://www.geeksforgeeks.org/pythonpath-environment-variable-in-python/) variable and still get your program working. 

In [Linux](https://www.geeksforgeeks.org/introduction-to-linux-operating-system/), we can use the following command in the terminal to set the path:

> export PYTHONPATH=’path/to/directory’

In the Windows system :

> SET PYTHONPATH=”path/to/directory”

To see if the PYTHONPATH variable holds the path of the new folder, we can use the following command:

> [echo](https://www.geeksforgeeks.org/echo-command-in-linux-with-examples/) $PYTHONPATH



## More about the package write format

Organize your project into packages and modules in the way below and use the `__init__.py` file properly:

```text
workspace/
|-- myProjName/
|   |-- __init__.py
|   |
|   |-- myFuns/
|   |   |-- __init__.py
|   |   |-- functions.py
|   |
|   |-- myCode/
|       |-- __init__.py
|       |-- main.py
```

The `__init__.py` files are **required** for Python to treat the directories as packages.

So you can use the **Example Code** to organize your proj. development:

```python
#----- functions.py (inside myFuncs package) -----
    def my_function():
        print("Hello from my_function!")

#----- main.py (inside myCode package) -----------
    from myProj.myFuncs.functions import my_function

    def main():
        my_function()

    if __name__ == "__main__":
        main()
```

### But how to write the `__init__.py` file

The `__init__.py` file is a special file in a Python package. It can be empty or contain Python code. The presence of this file in a directory tells Python that the directory should be treated as a package, and it allows you to use import statements to access modules or subpackages within that directory.

As for the double underscores in `__init__.py`, it's a Python convention to use double underscores `__` for special names or methods.

- An empty `__init__.py` file indicates that there is no special setup needed when the package is imported.

- You can include code in the `__init__.py` file. This code will be executed when the package is imported.
  
  - ```python
    # Inside myFuncs package
    # myFuncs/__init__.py
    
    print("Initializing myFuncs package")
    
    # You can import specific modules to make them available when the package is imported
    from . import functions
    ```
  
  - With this `__init__.py`, when you import the package, the print statement will execute, and the `functions` module will be available as part of the package.
  
  - the `__` indicate special names, and they are not required for every file or variable name. They are used to avoid unintentional name clashes with user-defined names.

---
layout: post
title: 'Install Python on Mac (Pip & venv)'
date: 2018-04-16 10:31:00 +0800
category: from_cnblogs
slug: p20180416103100
---
# Install Python (Pip & venv)
标签（空格分隔）： 运维


---

在**VSCode**中使用`/usr/bin/python3`创建一个虚拟环境(venv)，并在该虚拟环境中安装所需依赖包，步骤如下：

1. **创建虚拟环境**
首先，打开VSCode中的终端并导航到项目目录（即包含main.py脚本的目录）。然后运行以下命令以使用/usr/bin/python3创建虚拟环境：

```bash

/usr/bin/python3 -m venv my_venv
```

这将在当前目录中创建一个名为`my_venv` 的虚拟环境。


2. **激活虚拟环境**
在终端中激活虚拟环境：

如果你使用的是Linux或macOS，运行以下命令：

```bash

source my_venv/bin/activate
```
如果你使用的是Windows，运行以下命令：

```bash

.\my_venv\Scripts\activate
```
激活成功后，你会在终端提示符前看到 (my_venv) 的标志，表示你已经进入虚拟环境。

3. **安装所需依赖包**
激活虚拟环境后，在终端中运行以下命令来安装所需的依赖包（如 pandas 和 openpyxl）：

```bash

pip install pandas openpyxl
```
这会在虚拟环境中安装pandas和openpyxl包。

4. **在VSCode中选择虚拟环境**
打开VSCode后，按下Ctrl + Shift + P（macOS上是Cmd + Shift + P），然后输入Python: Select Interpreter，并选择它。
在弹出的列表中，选择你刚刚创建的虚拟环境。通常会显示为./my_venv/bin/python或类似路径。
选择成功后，VSCode将在编辑器的底部状态栏中显示当前使用的Python解释器。

5. **运行脚本**
现在你可以在VSCode中打开main.py脚本，并在终端中运行：

```bash
python main.py
```
或直接在VSCode中按F5启动调试，脚本会在你刚刚设置的虚拟环境中运行。

这样，你就可以使用虚拟环境来运行和管理你的Python项目。
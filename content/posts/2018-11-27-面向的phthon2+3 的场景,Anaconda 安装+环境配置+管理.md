---
layout: post
title: '面向的phthon2+3 的场景,Anaconda 安装+环境配置+管理'
date: 2018-11-27 03:34:00 +0800
category: from_cnblogs
slug: p20181127033400
---
### standard procedure in pyCharm for creating environment when Anaconda installed

#### Create a conda env via pyCharm

1. open pyCharm 
2. `create new project` -- select **Conda** as New env using.
   - Locations: ~\Anaconda_x\envs\py3env 
   - PythonVersion: python3.6   (python2.x as alternate)
   - Conda Excutable: ~\Anaconda_x\Scripts\conda.exe
   - :ballot_box_with_check:⊙Make available to all projects

3. you can open terminal tab(`Alt `+ `F12`) in pyCharm and manage the packages using `conda` commands. For instance, run the commands below for installing the `numpy` package:
   - `activate py3env`  
   - `conda list numpy`
   - `conda install numpy=1.15.1`


Test python 3 code:

```python
import numpy as np
arr = np.array([1,2,3])
print('python3')
print(arr)
```

The UI for configuring python2.7 environment is as below:

![image](https://www.cnblogs.com/images/cnblogs_com/sonictl/1350627/o_%e6%8d%95%e8%8e%b7.png)

**So that next time when creating new project, just select the existing interpreter for py2 or py3.**
**And launch the third-party py code via the cmd line in the help with `conda` or `activate` for Py2 or Py3 environments**
enjoy~ 


# Anaconda 安装及初始环境配置

- 面向的phthon2+3 的场景（需求很大）
- 安装+环境变量配置



### Anaconda 安装到win10

(略)

**测试一下**：

打开 cmd, 运行命令` conda  --version `，如果说“command no found”,说明要配环境变量：

​	```windows 环境变量配置:```

```
我的电脑 - 属性 - 高级系统设置 - 环境变量 - 系统变量 - Path - 编辑 - 
加入： C:\Users\sonic\Anaconda2
C:\Users\sonic\Anaconda2\Scripts
(默认是以上2条，如果你改了就改了它们。)一个是pythonx.exe的目录，一个是 conda.exe 的path
再测试一下 cmd 里的conda命令。
```



### 相关conda命令，用于管理环境

Ref: **Mac, Linux + windows** 都有，原文地址：<http://conda.pydata.org/docs/test-drive.html>

在cmd命令行中，（不是powerShell）

`conda info -e`  -- 查看环境，当前环境用*或() 标注。

`conda create --name envName numpy package2 package3 ...  ` -- 创建一个环境，位置在.../envs/envName

**激活环境**

```
Linux，OS X: source activate snowflakes
Windows：activate snowflake 
注： deactivate 是用于注销当前环境。
```

**创建不同python3的环境**

```
conda create -n snakes python=３
·Linux，OS X：source activate snakes
·Windows： activate snakes
```
**注意：如果你想在这个环境中使用pip，请分别在各自环境里安装pip: `conda install pip`。否则mac osx将使用自带的。**

**删除一个环境**

``` 
conda remove -n flowers --all
conda info -e
conda remove -h  #help about remove cmd
```



### 管理包packages/modules

**查看该环境中的包、安装、搜索**

```
conda list
conda install --name bunnies beautifulsoup4
conda install numpy=1.15.4
conda search beautifulsoup4

```

### 通过pip命令来安装包

对于那些无法通过conda安装或者从Anaconda.org获得的包，通常用pip来安装。

```
pip install see
conda list
```



-------------

别的一些：

##  移除包、环境、或者conda

如果你愿意的话。让我们通过移除一个或多个试验包、环境以及conda来结束这次测试指导。

### 移除包

假设你决定不再使用商业包IOPro。你可以在bunnies环境中移除它。

```
conda remove -n bunnies iopro
```

### 确认包已经被移除

使用conda list命令来确认IOPro已经被移除了

```
conda list
```

### 移除环境

我们不再需要snakes环境了，所以输入以下命令：
 conda remove -n snakes --all

### 确认环境被移除

为了确认snakes环境已经被移除了，输入以下命令：

```
 conda info --envis
```

snakes不再显示在环境列表里了，所以我们知道它已经被删除了

### 删除conda

- Linux，OS X：
   移除Anaconda 或 Miniconda 安装文件夹

```
rm -rf ~/miniconda OR  rm -rf ~/anaconda
```

- Windows：
   去控制面板，点击“添加或删除程序”，选择“Python2.7（Anaconda）”或“Python2.7（Miniconda）”并点击删除程序。



修改自：

作者：NorthPenguin

链接：https://www.jianshu.com/p/d2e15200ee9b

來源：jianshu.com

著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
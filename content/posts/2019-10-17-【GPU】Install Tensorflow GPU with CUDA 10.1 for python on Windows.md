---
layout: post
title: '【GPU】Install Tensorflow GPU with CUDA 10.1 for python on Windows'
date: 2019-10-17 10:04:00 +0800
category: from_cnblogs
slug: p20191017100400
---
# How to install Tensorflow GPU with CUDA 10.1 for python on Windows
## 在cuda 10.0的windows上安装Tensorflow GPU, python
ref: https://www.pytorials.com/how-to-install-tensorflow-gpu-with-cuda-10-0-for-python-on-windows/
But the above link is too complicated and the success is not garanteed.

## Before start, Notations neet to know: 
20190825: Note the cuda10.1 is NOT supported by pyTorch by now. Want pyTorch? recommend: [cuda10.0](https://developer.nvidia.com/cuda-toolkit-archive)

## Install Tensorflow GPU with CUDA 10.1 for python on Windows
**Tasks （四位爷）:**
 1. Install visual studio
 2. Install Cuda (i.e., Cuda ToolKit)
 3. Install cuDNN
 4. Install tensorflow

那么问题是，**这四位爷的版本得对上。** 所以就有人做了这个东西：https://github.com/fo40225/tensorflow-windows-wheel
里面详细列出来每个安装包对应的四位爷的版本。follow [this link](https://github.com/fo40225/tensorflow-windows-wheel) to strictly keep the fix of versions.

For example：
1.14.0\py37\GPU\cuda101cudnn76avx2	VS2019 16.1	10.1.168_425.25/7.6.0.64	AVX2	Python 3.7/Compute 3.0,3.5,5.0,5.2,6.1,7.0,7.5

#### 解释一下：
 - 第一列： the path of whl install package file, in this github repository. 
 - 第二列： Version of Compiler you need
 - 第三列： CUDA=10.1.168_425.25， cuDNN=7.6.0.64
 - 第四列： SIMD，即你的CPU支持的指令集是否支持AVX2. The tool `coreinfo` can tell you. get to know it by google.
 - 第五列： python版本，your GPU compute capabilities。ref: https://developer.nvidia.com/cuda-gpus

### 按照这5列的信息，就一步步装四位爷。
 - 装 visual studio
 - 装 Cuda
 - 装 cuDNN
 - 装 tensorflow
 
**第一位爷**，装 visual studio：上microsoft visual studio去下载安装vs 2019, 需了解tf是c++写的。[下载其他版本](https://visualstudio.microsoft.com/zh-hans/vs/older-downloads/)

**第二位爷**，装 Cuda 10.1， 上nvidia去下载安装即可。(20190825: 注意cuda10.1 暂时不被pyTorch支持，如果有pyTorch需求的，建议[cuda10.0](https://developer.nvidia.com/cuda-toolkit-archive))
Check the version number in the CMD terminal after installing：`nvcc --version`
  You will see something like this:
  ----
  >nvcc: NVIDIA (R) Cuda compiler driver
  Copyright (c) 2005-2019 NVIDIA Corporation
  Built on Wed_Apr_24_19:11:20_Pacific_Daylight_Time_2019
  Cuda compilation tools, release 10.1, V10.1.168
  
**第三位爷**，装cuDNN, 
Goto https://developer.nvidia.com/cudnn (Membership required) After login Download the following: cuDNN v7.6.0.64 Library for Windows [your version] for me Windows 10 Goto downloaded folder and extract cudnn-10.0-windows10-x64-v7.3.1.20.zip Go inside extracted folder and copy all files and folder from cuda folder (Bin, include, lib) and paste them to “C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1”. 
对应的文件放在对应的所在文件夹中。
参考： https://zhuanlan.zhihu.com/p/49832216

**第四位爷**：装tensorflow-gpu
建立一个virtual environment with python=3.7 用conda。指定env的名字py3env-gpu, python版本=3.7， 顺带装上包：pip six numpy wheel mock 等。cmd中运行：

  `conda create -n py3env-gpu python=3.7 pip six numpy wheel mock`
   
参考上述‘第一列： 在github这个项目里，whl安装包的路径’
Download tensorflow-gpu的whl file，from this github: https://github.com/fo40225/tensorflow-windows-wheel/tree/master/1.14.0/py37/GPU/cuda101cudnn76avx2

这里面是001和002的压缩分卷，全下载解压。

激活刚创建的conda_virtual_env:  `activate py3env-gpu`

cmd里cd到001-002的分卷解压路径，用`pip install tensorflow_gpu-1.14.0-cp37-cp37m-win_amd64.whl` 安装tensorflow-gpu，就完成了。

途中如果遇到connection的问题，用privoxy开个http代理，再在cmd里配一下代理服务器即可。参考：https://www.cnblogs.com/sonictl/p/11248627.html

---

**个人经验，仅供参考**，致敬：https://zhuanlan.zhihu.com/p/49832216

---
## 附：pyTorch install on Win10 x64 with GPU
目的是要在一个conda_env里既有tensorFlow 也有pyTorch，这样对于tf的代码和pytorch写的DNN代码都能运行在GPU上了。
### 老规矩，这次是五位爷：
 - Visual Studio 2017 （e.g. mu_visual_studio_community_2017_x86_x64_10049782.exe）
 - Cuda 10.0 （cuda_10.0.130_411.31_win10.exe）
 - cuDNN （cudnn-10.0-windows10-x64-v7.3.1.20）
 - tf (tensorflow_gpu-1.12.0-cp36-cp36m-win_amd64.whl from [this link](https://github.com/fo40225/tensorflow-windows-wheel))
 - pyTorch (ref [official cite](https://pytorch.org/get-started/locally/), use command:`conda install pytorch torchvision cudatoolkit=10.0 -c pytorch`)

`1.12.0\py36\GPU\cuda100cudnn73avx2	VS2017 15.8	10.0.130_411.31/7.3.1.20	AVX2	Python 3.6/Compute 3.0,3.5,5.0,5.2,6.1,7.0,7.5`

### 第一位： Visual Studio 2017
 这个竟然费了点周折，MS官网竟然扭扭捏捏，别的渠道网盘获取的。

### 第二位： CUDA 10.0.130
 从CUDA10.1 downgrade注意卸载CUDA相关的版本号为10.1的条目，并卸载NSight相关的，比如NSight for visual studio 2019.
   再装即可。

### 第三位： cuDNN 10.0 - v7.3.1.20
 去官网下载对应版本即可。

### 第四位：tensorFlow
 github那里去下载whl文件，注意版本对应，此处用的python 3.6的
 建virtual_env: `conda create -n py36env-gpu python=3.6 pip six numpy wheel mock gensim`
 activate 这个新建的virtual env
 cd 到 whl文件的目录
 pip install xxx.whl 进行安装。

### 第五位：pyTorch
    Follow official site: [official cite](https://pytorch.org/get-started/locally/)

---
## Some discussions:
   Can one cmd solve all problems? [link](https://stackoverflow.com/questions/54689096/install-keras-for-gpu)

>once you have Anaconda installed, you simply need to create a new environment where you want to install keras-gpu and execute the command:
  `conda install -c anaconda keras-gpu`
This will install Keras along with both tensorflow and tensorflow-gpu libraries as the backend. (There is also no need to install separately the CUDA runtime and cudnn libraries as they are also included in the package - tested on Windows 10 and working. Ensure your TensorFlow/Keras environment is using Python 3.6.)

If you're interested in this, have a try.

-----

# 在Windows10中配置Keras-GPU版的环境

* 要安装的是keras-GPU
* 在anaconda navigator右侧选择搜索tensorflow
* 在anaconda中安装tensorflow的好处就是他会自动的帮你自动安装好CUDA和CUDAnn。具体如下：
* 1. 安装:  tensorflow-gpu
* 2. 然后安装：Keras-GPU。
* 如果顺利的话。
* 如果你使用了最新版本的Anaconda，在安装tensorflow的时候，anaconda会帮我们把python版本回退到3.6.
* 检查一下你的cuda版本。在andaconda软件中，所示cuda。查看cudatoolkit的版本。最好把你的cudatoolkit的版本保持和你的显卡cuda版本一致，或者比cuda低一个版本。
* 我的cudatoolkit是9.0的版本，我的cuda版本是10.0，使用cudatoolkit9.0还是可以的。
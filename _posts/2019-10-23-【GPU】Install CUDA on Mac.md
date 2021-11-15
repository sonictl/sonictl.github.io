---
layout: post
title: '【GPU】Install CUDA on Mac'
date: 2019-10-23 13:23:00 +0800
category: from_cnblogs
slug: p20191023132300
---
# Install CUDA on Mac

*This instruction is following the **Online Documentation** for CUDA Toolkit.*

Some notifications for tf-gpu installation:
 - After tf 1.2, the `source code building` becomes the only way of installation of TensorFlow-GPU in Mac OSX. [link](https://stackoverflow.com/questions/44744737/tensorflow-mac-os-gpu-support) And tensorFlow nolonger support GPU on OSX. 这带来了很多安装的不确定性和难度。
 - 从源码安装是可能的途径，但是有人制作whl放在网上供下载。[github link](https://github.com/TomHeaven/tensorflow-osx-build/releases)
 - python=3.6.5 CUDA=9.2.148 cuDNN=7.1.4 tf-gpu=1.9 可参考的一个配置选项，来自pyTorch社区

**1.1 系统要求：**

   Cuda 兼容的 GPU
   OSX 10.13
   Clang 编译器 和 toolchain 安装自Xcode
   CUDA Toolkit:   https://developer.nvidia.com/cuda-toolkit-archive  Guided by: [Online Documentation](https://docs.nvidia.com/cuda/archive/9.2/)

Xcode版本 9.2， Apple LLVM版本9.0.0, 如何检查：

```
/usr/bin/cc --version
```

如何下载：https://developer.apple.com/download/more/

**1.2 步骤总览**

​    1.2.1 check the requirements.
​    1.2.2 Download and Install
​    1.2.3 Verification
​    1.2.4 Install CuDNN: [CuDNN Archieve](https://developer.nvidia.com/rdp/cudnn-archive)
​    1.2.5 Install tf-gpu

**1.3 Check the requirements**

**1.4 Download and Install**

	- CUDA Driver
	- CUDA Toolkit
	- CUDA Samples

1.5 Verification for installing of CUDA Toolkit

1.6 Install CuDNN

​    [Download cuDNN v7.1.4 (May 16, 2018), for CUDA 9.2](https://developer.nvidia.com/rdp/cudnn-archive#a-collapse714-92)

​    [cuDNN v7.1.4 Library for OSX](https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.1.4/prod/9.2_20180516/cudnn-9.2-osx-x64-v7.1)



1.7 Install tf-gpu



More References:

   https://darienmt.com/self-driving-cars/2017/06/07/tensorflow-with-gpu-on-your-mac.html
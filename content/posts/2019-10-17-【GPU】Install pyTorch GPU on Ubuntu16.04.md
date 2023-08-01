---
layout: post
title: '【GPU】Install pyTorch GPU on Ubuntu16.04'
date: 2019-10-17 10:06:00 +0800
category: from_cnblogs
slug: p20191017100600
---


**Note:** Contents below may out-of-date. But the procedures are referable.



**Avaliable version combinations for pyTorch/Tensorflow-gpu:**

```
python=3.6.5  CUDA=9.2.148  cuDNN=7.1.4  tf-gpu=1.9

python=3.6.5  CUDA=9.1.85   cuDNN=7.1.3  tf-gpu=1.8
              CUDA=9.0.176  cudnn=7.3.1.20   tensorflow-gpu=1.12.0
              CUDA=9.0;     cuDNN=7.4.1      tensorflow-gpu=1.12.0； 
              cuda=9.0;     cudnn=7.5.0      tensorflow=1.8.0可以用的

python=3.6.7  cuda=9.0;     cuDNN=7.4.2      tensorflow-gpu = 1.9.0    pytorch = 1.0.0
              cuda=9.0;     cudnn=7.0.5

```

=================


=================
Install: CUDA=9.0  CUDNN=7.0.5
http://www.twistedwg.com/2018/06/15/cuda9_cudnn7.html
https://blog.csdn.net/lukaslong/article/details/81488219

=================
注意python3, pip3, Follow pyTorch web: https://pytorch.org/get-started/locally/

1. UPDATE:
  `sudo apt update`
  `sudo apt upgrade`

2. curl
  `sudo apt-get install curl`
  or:
  `sudo apt install curl`
  check:
  `curl --version`

3. Anaconda
  `curl -O https://repo.anaconda.com/archive/Anaconda3-2019.07-Linux-x86_64.sh`
  `sh Anaconda3-2019.07-Linux-x86_64.sh`

4. pip3(Optional)
  `sudo apt-get -y install python3-pip`
  or:
  `sudo apt install python3-pip`

5. Create Env:
  `conda create -n py36gpu python=3.6 numpy pip six wheel mock`
  `source activate py36gpu`

6. For CUDA=9.0, Install pyTorch:
  `conda install pytorch torchvision cudatoolkit=9.0 -c pytorch`
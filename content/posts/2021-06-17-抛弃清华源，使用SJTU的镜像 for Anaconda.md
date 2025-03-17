---
layout: post
title: '抛弃清华源，使用SJTU的镜像 for Anaconda'
date: 2021-06-17 13:25:00 +0800
category: 运维
slug: p20210617132500
---
##   	[使用SJTU的Anaconda镜像](https://verrickt.github.io/2020/12/12/conda-mirror-from-sjtu/)  

[清华TUNA](https://mirrors.tuna.tsinghua.edu.cn/)的Anaconda镜像不稳，好的时候一切顺利，坏的时候一个24K的包下5分钟；Anaconda的官方源则更惨，连repo.json都下不下来；
今天又是清华的源大姨妈的一天：Pytorch下载到100MB就超时，折腾了三个小时也装不上。而同在华东的[SJTU](https://mirrors.sjtug.sjtu.edu.cn/)的镜像就很稳定，这次全线换成SJTU啦。

首先执行

```
conda config --set show_channel_urls yes
```



在`/usr`下生成`.condarc`文件，然后将其内容替换为

```
channels:
  - defaults
show_channel_urls: true
channel_alias: https://anaconda.mirrors.sjtug.sjtu.edu.cn
default_channels:
  - https://anaconda.mirrors.sjtug.sjtu.edu.cn/pkgs/main
  - https://anaconda.mirrors.sjtug.sjtu.edu.cn/pkgs/free
  - https://anaconda.mirrors.sjtug.sjtu.edu.cn/pkgs/r
  - https://anaconda.mirrors.sjtug.sjtu.edu.cn/pkgs/pro
  - https://anaconda.mirrors.sjtug.sjtu.edu.cn/pkgs/msys2
custom_channels:
  conda-forge: https://anaconda.mirrors.sjtug.sjtu.edu.cn/cloud
  msys2: https://anaconda.mirrors.sjtug.sjtu.edu.cn/cloud
  bioconda: https://anaconda.mirrors.sjtug.sjtu.edu.cn/cloud
  menpo: https://anaconda.mirrors.sjtug.sjtu.edu.cn/cloud
  pytorch: https://anaconda.mirrors.sjtug.sjtu.edu.cn/cloud
  simpleitk: https://anaconda.mirrors.sjtug.sjtu.edu.cn/cloud
```

再运行`conda clean -i`清除索引缓存即可。
终于装上Pytorch了，可喜可贺😂
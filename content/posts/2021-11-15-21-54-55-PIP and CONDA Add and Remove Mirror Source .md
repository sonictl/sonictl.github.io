---
layout: post
title:  "PIP and CONDA Add and Remove Mirror Source 配置软件源"
date: 2021-11-15 21:55:30 +0800
categories: jekyll
slug: p20211115215455
---
**First, PIP**

1, add a source

For example, add SJTU Source https://mirror.sjtu.edu.cn/pypi/web/simple：

```
pip config set global.index-url https://mirror.sjtu.edu.cn/pypi/web/simple
```

If SET has multiple times, it seems to be only saved the mirror source of the last set.

2, delete the source

```
pip config unset global.index-url
```

3, check which source now

```
pip config list
```

 

**Second, Conda**

1, add a source

For example, Tsinghua Source:

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes

```
SJTU:
```
conda config --add channels https://mirror.sjtu.edu.cn/anaconda/{{pkg_name}}

e.g.
conda config --add channels https://mirror.sjtu.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirror.sjtu.edu.cn/anaconda/pkgs/main
conda config --add channels https://mirror.sjtu.edu.cn/anaconda/pkgs/mro
conda config --add channels https://mirror.sjtu.edu.cn/anaconda/pkgs/msys2
pkgs/pro: (deprecated) conda config --add channels https://mirror.sjtu.edu.cn/anaconda/pkgs/pro
pkgs/r: (empty) conda config --add channels https://mirror.sjtu.edu.cn/anaconda/pkgs/r
```
[read more](https://mirrors.sjtug.sjtu.edu.cn/docs/anaconda)

 2, delete the source

```
conda config --remove-key channels
```

 3, See which sources are used now

```
conda config --show channels
```

Can also use

```
conda config --show-sources
```

 

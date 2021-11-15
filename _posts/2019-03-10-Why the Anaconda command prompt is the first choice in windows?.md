---
layout: post
title: 'Why the Anaconda command prompt is the first choice in windows?'
date: 2019-03-10 13:24:00 +0800
category: from_cnblogs
slug: p20190310132400
---
为什么在windows里，首选的conda命令行工具是Anaconda command prompt？
#### In windows, what's the difference between command prompt and anaconda prompt

**Anaconda command prompt** is just like **command prompt**, but it makes sure that you are able to use anaconda and conda commands from the prompt, without having to change directories or your path.

When you start Anaconda command prompt, you'll notice that it adds/("prepends") a bunch of locations to your PATH. These locations contain commands and scripts that you can run. So as long as you're in the Anaconda command prompt, you know you can use these commands.

> you can check your PATH by launch the `echo %PATH` in your **command prompt**(cmd.exe in windows) or **Anaconda command prompt**

During the installation of Anaconda there is a choice to add these to the PATH by default, and if checked you can also use these commands on the regular command prompt. But the anaconda prompt will always work. see the snapshot of installing anaconda below:

![](https://img2018.cnblogs.com/blog/780771/201903/780771-20190310211820740-842966755.png)

As far as updating conda, if it doesn't work in **command prompt**, you can launch

`conda update conda`

in **Anaconda command prompt**.

*for my backup:*

```%USERPROFILE%\Anaconda3;%USERPROFILE%\Anaconda3\Library\mingw-w64\bin;%USERPROFILE%\Anaconda3\Library\usr\bin;%USERPROFILE%\Anaconda3\Library\bin;%USERPROFILE%\Anaconda3\Scripts;%USERPROFILE%\Anaconda3\bin;
```shell
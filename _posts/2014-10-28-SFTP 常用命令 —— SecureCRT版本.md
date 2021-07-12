---
layout: post
title: 'SFTP 常用命令 —— SecureCRT版本'
date: 2014-10-28 03:37:00 +0800
category: from_cnblogs
slug: p20141028033700
---


<h4>SecureCRT按下ALT&#43;P就开启新的会话 进行ftp操作。</h4>
<br>
输入:help命令，显示该FTP提供所有的命令<br>
pwd: 查询linux主机所在目录(也就是远程主机目录)<br>
lpwd: 查询本地目录（一般指windows上传文件的目录：我们可以通过查看”选项“下拉框中的”会话选项“，如图二：我们知道本地上传目录为：D:/我的文档）<br>
ls: 查询连接到当前linux主机所在目录有哪些文件<br>
lls: 查询当前本地上传目录有哪些文件<br>
lcd: 改变本地上传目录的路径<br>
cd: 改变远程上传目录<br>
get: 将远程目录中文件下载到本地目录<br>
put: 将本地目录中文件上传到远程主机(linux)<br>
quit: 断开FTP连接
   

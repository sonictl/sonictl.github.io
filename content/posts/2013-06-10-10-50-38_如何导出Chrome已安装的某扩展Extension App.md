---
layout: post
title:  "如何导出Chrome已安装的某扩展Extension App"
date: 2013-06-10 10:50:38 +0800
tags: 
slug: p20130610105038
---

# 如何导出Chrome已安装的某扩展Extension App





如何导出Chrome已安装的某扩展以前用过的有些[Extend](https://so.csdn.net/so/search?q=Extend&spm=1001.2101.3001.7020) App，市场里面又找不到更好的怎么办？


这个时候就要用到导出了。


* 打开扩展程序Setting Extensions 界面，
* 选择开发人员模式Developer mode，找到该扩展，记住每个Extension下方的ID。
* 点“击打包扩展程序” ： Pack Extension
* 选择扩展路径C:\Users\当前用户名\AppData\Local\Google\Chrome\User Data\Default\Extensions\扩展ID\版本号，  
 这里要特别说明，虽然chrome提示是选择扩展根目录，但是如果你把目录选择在扩展ID这一层你就错了，你应该选择下一层的版本号这个目录。
* 然后点击确定，就有提示导出成功，并提示了保存位置，crx文件就可以拿来分享了。


  
 


  
 





---
layout: post
title: '干净win7要做几步才能运行第一个Spring MVC 写的动态web程序'
date: 2014-11-29 12:31:00 +0800
category: from_cnblogs
slug: p20141129123100
---


<br>
<br>
<span style="font-size:18px">干净win7要做几步才能运行第一个Spring MVC 写的动态web程序：</span><br>
<br>
<br>
1. 下载安装jdk<br>
2. 配置Java环境变量<br>
3. 测试一下第1，2两步是否完全成功：http://jingyan.baidu.com/article/14bd256e2e3e0cbb6d261201.html<br>
3. 下载安装Eclipse J2EE luna（一定要是j2EE版本，否则会有jar包缺失引起internal error，补起来很麻烦）<br>
4. 在Eclipse中安装Spring Tool Suite(不要纠结SpringIDE与SpringToolSuite的区别，STS就好。)<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Help &gt;&gt; Eclipse Marketplace &gt;&gt; Search: Spring Tool Suite &gt;&gt; Install the one that matches.<br>
5. 下载解压Tomcat: &nbsp;http://tomcat.apache.org/download-80.cgi<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;选择符合你机器的版本和&#26684;式进行下载，解压放在你喜欢的路径下。<br>
6. 配置tomcat环境变量：<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;配置Tomcat环境变量<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;1，新建变量名：CATALINA_BASE，变量&#20540;：C:\tomcat<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;2，新建变量名：CATALINA_HOME，变量&#20540;：C:\tomcat<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;3，打开PATH，添加变量&#20540;：%CATALINA_HOME%\lib;%CATALINA_HOME%\bin<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;参考：http://jingyan.baidu.com/article/8065f87fcc0f182330249841.html<br>
7. 为了能方便调试，Eclipse设置Tomcat运行环境：<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Window &gt;&gt; Preferences &gt;&gt; Server &gt;&gt; Runtime Environment &gt;&gt; Add... &gt;&gt; Select your tomcat Path &gt;&gt; OK<br>
8. Configure your tomcat in Eclipse:<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Run &gt;&gt; Run Configuration &gt;&gt; Right click &quot;Apache Tomcat&quot; &gt;&gt; new &gt;&gt; name: myTomcat &gt;&gt; OK<br>
9. 建立Spring MVC项目，总算来了：<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;File &gt;&gt; new &gt;&gt; Project &gt;&gt; Spring/Spring Project &gt;&gt; Project name, Templates:SpringMVC ...<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Eclipse已经预置了一个简单的Hello world代码结构，所以下一步是运行一下看看。<br>
10.运行Run Project<br>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Right click the project root in PackageExplorer on the right side of Eclipse's window shown in the image below:</p>
<p><img src="http://img.blog.csdn.net/20141129203056328?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="3" alt=""><br>
</p>
<p><br>
</p>
<p>&nbsp; &nbsp; Enjoy.&nbsp;</p>
   

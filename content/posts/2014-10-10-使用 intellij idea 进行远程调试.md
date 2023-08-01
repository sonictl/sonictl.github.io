---
layout: post
title: '使用 intellij idea 进行远程调试'
date: 2014-10-10 03:17:00 +0800
category: from_cnblogs
slug: p20141010031700
---


<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
转自：<a target="_blank" href="http://yiminghe.iteye.com/blog/1027707">http://yiminghe.iteye.com/blog/1027707</a></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
<br>
以前都是很土得打 log ，发现一篇关于&nbsp;<a target="_blank" target="_blank" href="http://www.ibm.com/developerworks/cn/java/j-lo-jpda1/" style="color:rgb(16,138,198)">java 调试器架构</a>&nbsp;，以及&nbsp;<a target="_blank" target="_blank" href="http://www.ibm.com/developerworks/cn/opensource/os-eclipse-javadebug/" style="color:rgb(16,138,198)">eclipse
 上使用</a>&nbsp;的文章，在常用的 intellij idea 以及 tomcat 上调试成功，结合调用堆栈希望可以加快 ``how tomcat works`` 读书进度。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
1. tomcat 7.0.5 启动支持调试</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
新建文件 setenv.bat</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<div class="dp-highlighter" id="" style="font-family:Monaco,'DejaVu Sans Mono','Bitstream Vera Sans Mono',Consolas,'Courier New',monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word; line-height:25.1875px">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Java代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://yiminghe.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-j" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left-width:1px; border-left-style:solid; border-left-color:rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">SET&nbsp;CATALINA_OPTS=-server&nbsp;-Xdebug&nbsp;-Xnoagent&nbsp;&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left-width:1px; border-left-style:solid; border-left-color:rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">-Djava.compiler=NONE&nbsp;-Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=<span class="number" style="color:rgb(192,0,0)">8000</span>&nbsp;&nbsp;</span></li></ol>
</div>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;放入 tomcat 下 bin/ 中（和 startup.bat）同级。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
2. 点击 startup.bat 启动，控制台输出调试支持日志表示成功配置：</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<div class="dp-highlighter" id="" style="font-family:Monaco,'DejaVu Sans Mono','Bitstream Vera Sans Mono',Consolas,'Courier New',monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word; line-height:25.1875px">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Java代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://yiminghe.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-j" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left-width:1px; border-left-style:solid; border-left-color:rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">Listening&nbsp;<span class="keyword" style="color:rgb(127,0,85); font-weight:bold">for</span>&nbsp;transport&nbsp;dt_socket&nbsp;at&nbsp;address:&nbsp;<span class="number" style="color:rgb(192,0,0)">8000</span>&nbsp;&nbsp;</span></li></ol>
</div>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
3. idea remote debug 配置</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
打开已有的 web 类型项目，设置运行配置</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
<img alt="" src="http://dl.iteye.com/upload/attachment/474974/34fbf300-7218-3cf4-b5d7-78b9071bcb06.png" style="border:0px"></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
找到 remote 子项，选择新增配置</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
<img alt="" src="http://dl.iteye.com/upload/attachment/474976/461a7e67-f3d3-33e4-b85a-3c6d0b86db24.png" style="border:0px"></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
输入项目名称，端口设置 tomcat 配置的 8000，并选择源码所在模块，调试模式为 attach</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
<img alt="" src="http://dl.iteye.com/upload/attachment/474978/c47eff3f-26f2-398c-8df0-4550df482116.png" title="点击查看原始大小图片" class="magplus" width="700" height="564" style="border:0px"></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
点击 ok 关闭设置窗口</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
4. 在源码上设置断点后，点击调试按钮</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
<img alt="" src="http://dl.iteye.com/upload/attachment/474980/23a53d6a-4bba-3d26-9859-626023704395.png" style="border:0px"></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
调试窗口输出</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<div class="dp-highlighter" id="" style="font-family:Monaco,'DejaVu Sans Mono','Bitstream Vera Sans Mono',Consolas,'Courier New',monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word; line-height:25.1875px">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Java代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://yiminghe.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-j" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left-width:1px; border-left-style:solid; border-left-color:rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">Connected&nbsp;to&nbsp;the&nbsp;target&nbsp;VM,&nbsp;address:&nbsp;<span class="string" style="color:blue">'localhost:8000'</span>,&nbsp;transport:&nbsp;<span class="string" style="color:blue">'socket'</span>&nbsp;&nbsp;</span></li></ol>
</div>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
表示正常连上了远端（localhost）服务器。</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
5.启动调试</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
使用浏览器访问对应服务器应用，启动调试，运行到客户端断点时，就可以查看当前帧变量与堆栈信息了：</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
<img alt="" src="http://dl.iteye.com/upload/attachment/474982/eaafaea7-47dd-3235-86db-acf366a0c12d.png" title="点击查看原始大小图片" class="magplus" width="700" height="346" style="border:0px"></p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
&nbsp;</p>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px; line-height:25.1875px">
再进一步关联&nbsp;<a target="_blank" target="_blank" href="http://tomcat.apache.org/download-70.cgi" style="color:rgb(16,138,198)">tomcat 源码</a>&nbsp;则可以了解到请求在 servlet 容器中的一系列转发过程了。</p>
   

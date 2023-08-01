---
layout: post
title: 'Spring MVC 学习笔记1 - First Helloworld by Eclipse【&amp; - java web 开发Tips集锦】'
date: 2014-10-23 09:45:00 +0800
category: from_cnblogs
slug: p20141023094500
---


<p>Spring MVC 学习笔记1 - First Helloworld by Eclipse<br>
</p>
<p>reference:http://www.gontu.org</p>
<p>1. 下载 Spring freamworks 4.0.0 RELEASE</p>
<br>
<br>
2. 下载 commons-logging-1.2-bin<br>
<br>
<br>
3. 在Eclipse Luna Service Release 1 (4.4.1)中新建Dynamic Web App<br>
<br>
<br>
4. 配置服务器平台：Window - show view - servers ; &quot;Click to creat server&quot;;Apache/Tomcat v8.0<br>
<br>
<br>
5. Tomcat installation directory; &quot;next&quot;; &quot;finish&quot;<br>
<span style="white-space:pre"></span>右键点击项目名，Properties, TargetRuntimes, 勾“Apache Tomcat v8.0”<br>
<br>
<br>
6. 添加jar文件入/WebContet/WEB-INF/lib&nbsp;<br>
<span style="white-space:pre"></span>从这里来：Spring freamworks 4.0.0 RELEASE &#43; commons-logging-1.2.jar<br>
<br>
<br>
7. 写第1个文件：web.xml<br>
<span style="white-space:pre"></span>a.它是截流器<br>
<span style="white-space:pre"></span>&lt;web-app&gt;<br>
<span style="white-space:pre"></span>b.&lt;display-name&gt;<br>
<span style="white-space:pre"></span>c.&lt;servlet&gt; - name - class<br>
<span style="white-space:pre"></span>d.&lt;servlet-mapping&gt; -name ,url-pattern<br>
<br>
<br>
8. 写第2个文件：$servletname-dispacher.xml$<br>
<span style="white-space:pre"></span>a.&lt;beans ...&gt;&lt;/beans&gt;<br>
<span style="white-space:pre"></span>b.&lt;bean id=HandlerMapping, ... /&gt;<span style="white-space:pre"></span>&lt;!--HandlerMapping--&gt;<br>
<span style="white-space:pre"></span>&lt;bean name=&quot;/welcome.html&quot; class=&quot;edu.bit.helloController&quot; /&gt;<br>
<span style="white-space:pre"></span>c.&lt;bean name=welcome.html, class=/&gt;<br>
<span style="white-space:pre"></span>d.&lt;bean id=&quot;viewResolver&quot;<span style="white-space:pre"></span>&lt;!--ViewResolver&gt;<br>
<span style="white-space:pre"></span><br>
9. 写第3个文件：helloController.java<br>
&nbsp; &nbsp;在HanderMapping 中指定了 Controller： JavaResources/src/edu.bit.helloController.java<br>
&nbsp; &nbsp;creat package &quot;edu.bit&quot; at JavaResources/src<br>
&nbsp; &nbsp;creat class file: helloController.java<br>
<br>
<br>
<span style="white-space:pre"></span>写HelloController.class<br>
<br>
<br>
10.写Hellopp.jsp<br>
<span style="white-space:pre"></span>helloController.java中指定了：<br>
<span style="white-space:pre"></span>ModelAndView modelandview = new ModelAndView(&quot;Hellopp&quot;);<br>
<span style="white-space:pre"></span>故需要 Hellopp.jsp 作为ViewResolver的解析目标<br>
<br>
<br>
11. Run - Run As - Run on Server<br>
<br>
<p><span style="font-family:Microsoft YaHei; font-size:18px; color:#ff0000"><strong>【Java Web开发Tips集锦】</strong></span></p>
<p><span style="font-family:Microsoft YaHei; font-size:18px; color:#ff0000"><strong>1. 特别注意</strong>，在日后的学习中，每次修改了代码再run之前，最好Project&gt;&gt;clean..以后，再run</span></p>
<span style="font-family:Microsoft YaHei; font-size:18px; color:#ff0000">2. eclipse新建jsp页面默认编码设置UTF-8:&nbsp;</span>
<pre id="best-content-354770037" class="best-text mb-10" style="margin-top:0px; margin-bottom:10px; padding:0px; font-family:arial,'courier new',courier,宋体,monospace; white-space:pre-wrap; word-wrap:break-word; color:rgb(51,51,51); font-size:14px; line-height:24px; background-color:rgb(241,254,221)">Window→preferences→General→Content Types,然后打开右边Text选中JSP，在下面Default encoding：那里输入编码，然后点击Update ，ok，
window - preferences - Web - JSP Files页 - Creating files框，Encoding选项： UTF - 8 .点&quot;Apply&quot;就行了。</pre>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
   

---
layout: post
title: 'Spring MVC 学习笔记11 —— 后端返回json格式数据'
date: 2014-11-25 03:21:00 +0800
category: from_cnblogs
slug: p20141125032100
---


<h3>Spring MVC 学习笔记11 —— 后端返回json&#26684;式数据</h3>
<h3>&nbsp;我们常常听说json数据，首先，什么是json数据，总结起来，有以下几点：</h3>
<p><span style="white-space:pre"></span>&nbsp; &nbsp;1. JSON的全称是”JavaScript Object Notation”，意思是JavaScript对象表示法.<br>
<span style="white-space:pre"></span>&nbsp; &nbsp;2.&nbsp;它是一种基于文本，独立于语言的轻量级数据交换&#26684;式.<br>
<span style="white-space:pre"></span><span style="white-space:pre"></span>&nbsp; &nbsp;3. json的两种结构：对象{key:value,key:value,...} &nbsp;和 &nbsp;数组 [value, value2, ... ]<br>
<span style="white-space:pre"></span>&nbsp; &nbsp;4. json字符串：普通字符串、json字符串、json对象的区别<br>
<span style="white-space:pre"></span>&nbsp; &nbsp;5. 不同编程工具使用json的方法</p>
<p>&nbsp;参考：<a target="_blank" target="_blank" href="http://www.cnblogs.com/mcgrady/archive/2013/06/08/3127781.html">http://www.cnblogs.com/mcgrady/archive/2013/06/08/3127781.html</a></p>
<p><br>
</p>
<h3>Spring MVC 返回json数据，用show来实现：</h3>
<p>UserController.java:</p>
<p>保持两套请求，一套是传统的请求返回数据；一套是请求json&#26684;式返回数据。</p>
<p></p>
<pre code_snippet_id="532140" snippet_file_name="blog_20141125_1_1513179"  code_snippet_id="532140" snippet_file_name="blog_20141125_1_1513179" name="code" class="java">	//6. 查一个用户 show.jsp
	@RequestMapping(value=&quot;/{username}&quot;, method=RequestMethod.GET)
	public String show(@PathVariable String username, Model model){
		model.addAttribute(&quot;user1&quot;,users.get(username));	//user1参数属性名，到了视图，就是user1
		return &quot;user/show&quot;;
	}
	
	//6. 查一个用户 show.jsp
	@RequestMapping(value=&quot;/{username}&quot;, method=RequestMethod.GET, params=&quot;jsoon&quot;)
	@ResponseBody	//这里要加一行
	public User show(@PathVariable String username){      //声明中没有了Model
		//model.addAttribute(&quot;user1&quot;,users.get(username));
		return users.get(username);            //不返回String了，返回user对象
	}</pre><br>
<strong>说明</strong>，以上代码中：
<p></p>
<p>&nbsp; 1. 增加@ResponseBody,&nbsp;<br>
&nbsp; 2. no return String, but User Object<br>
&nbsp; 3. no Model<br>
&nbsp; 4. directly return users.get(username)//注意返回的是一个User对象<br>
&nbsp; 5. 可以在RequestMapping行增加一个请求&#20540;：Params=&quot;jsoon&quot;//这是规定如要进这个方法返回json，需要带个参数jsoon<br>
</p>
<p>此时可测试，返回406错误如图：（通过url加入?jsoon访问了，但没有头文件为它进行解释）</p>
<p><img src="http://img.blog.csdn.net/20141125111809515?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""><br>
</p>
<p>所以我们需要加入一个jar包：jackson-all-1.x.x.jar</p>
<p>从这个网址可以下载到：http://jarfiles.pandaidea.com/jackson.all.html</p>
<p>把jar包复制到项目文件夹/lib文件夹下，重新debug，链接进入：http://localhost:8080/myhello/user/sdy?jsoon</p>
<p>此时，有的浏览器Chrome，firefox等能显示如下，其他浏览器如360等，会提示下载sdy.json文件，notepad&#43;&#43;打开以后也是如下内容。<br>
</p>
<p><img src="http://img.blog.csdn.net/20141125135446640?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""><br>
</p>
<p>Enjoy ;)</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
   

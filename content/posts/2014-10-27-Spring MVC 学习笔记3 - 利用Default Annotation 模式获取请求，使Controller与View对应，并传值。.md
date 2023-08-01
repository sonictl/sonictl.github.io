---
layout: post
title: 'Spring MVC 学习笔记3 - 利用Default Annotation 模式获取请求，使Controller与View对应，并传值。'
date: 2014-10-27 03:14:00 +0800
category: from_cnblogs
slug: p20141027031400
---


<p>1. WEB-INF/web.xml<span style="white-space:pre"> </span>这里定义了获取请求后，执行的第一步。抓取请求。<br>
</p>
<pre code_snippet_id="497959" snippet_file_name="blog_20141027_1_2374591"  code_snippet_id="497959" snippet_file_name="blog_20141027_1_2374591" name="code" class="html">	&lt;servlet&gt;
		&lt;servlet-name&gt;appServlet&lt;/servlet-name&gt;
		&lt;servlet-class&gt;org.springframework.web.servlet.DispatcherServlet&lt;/servlet-class&gt;
		&lt;init-param&gt;
			&lt;param-name&gt;contextConfigLocation&lt;/param-name&gt;
			&lt;param-value&gt;/WEB-INF/spring/appServlet/servlet-context.xml&lt;/param-value&gt;
		&lt;/init-param&gt;
		&lt;load-on-startup&gt;1&lt;/load-on-startup&gt;
	&lt;/servlet&gt;
</pre><br>
<br>
2. WEB-INF/spring/appServlet/servlet-context.xml<span style="white-space:pre"> </span>
这里定义了Dispatcher Servlet模式<br>
<pre code_snippet_id="497959" snippet_file_name="blog_20141027_2_6027898"  code_snippet_id="497959" snippet_file_name="blog_20141027_2_6027898" name="code" class="html">	&lt;!--Default Annotation model- Dispacher Servlet--&gt;
	&lt;!-- Enables the Spring MVC @Controller programming model --&gt;
	&lt;annotation-driven /&gt;


	&lt;!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory --&gt;
	&lt;resources mapping=&quot;/resources/**&quot; location=&quot;/resources/&quot; /&gt;</pre><br>
<br>
3. src.main.java.edu.bit.myhello/HelloController.java<br>
<span style="white-space:pre"></span>//Controller文件：处理请求，完成操作。<br>
<pre code_snippet_id="497959" snippet_file_name="blog_20141027_3_694961"  code_snippet_id="497959" snippet_file_name="blog_20141027_3_694961" name="code" class="java">package edu.bit.myhello;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;


@Controller//@Controller注入
public class HelloController {
	
	//表示用哪个url请求来对应本方法：
	@RequestMapping({&quot;/hello&quot;,&quot;/&quot;})
	public String hello(){
		System.out.println(&quot;Hello, HelloController&quot;);
		return &quot;hello00&quot;;	//返回一个hello字串，会调用WEB-INF/views/hello00.jsp进行显示。
	}
}</pre>
<p></p>
<p><br>
<br>
</p>
<h3>Spring MVC 传&#20540;(一)</h3>
<h4>1. 从视图传&#20540;给Controller</h4>
<span style="white-space:pre"></span>Internal View Resolver 方法, 通过函数的参数来传递。<br>
<br>
在HelloController.java文件中，<br>
-----------<br>
<p></p>
<pre code_snippet_id="497959" snippet_file_name="blog_20141027_4_6070523"  code_snippet_id="497959" snippet_file_name="blog_20141027_4_6070523" name="code" class="java">	@RequestMapping({&quot;/hello&quot;})
	public String hello(String stringIn){	//此时在浏览器中输入 {url}/hello?stringIn=AAA
	System.out.println(stringIn);
	return &quot;hello&quot;;	
	}</pre>==========<br>
<span style="white-space:pre"></span>
<p><span style="white-space:pre"></span>可见，有 public String hello(String stringIn) ，通过函数参数传&#20540;给Controller的方法。<br>
<span style="white-space:pre"></span>这里介绍使用@RequestParam来传&#20540;给Controller。<br>
<br>
<span style="white-space:pre"></span>在src/main/java/.../HomeController.java中<br>
<span style="white-space:pre"></span></p>
<pre code_snippet_id="497959" snippet_file_name="blog_20141027_5_394786"  code_snippet_id="497959" snippet_file_name="blog_20141027_5_394786" name="code" class="java">	@RequestMapping({&quot;/view2controller&quot;})	// ?inString=AAA,通过这种方式传值、请求
	public String view2controller(@RequestParam(&quot;inString&quot;) String reString){
				   //(@RequestParam(&quot;inString&quot;) String reString)
		System.out.println(reString);
		return &quot;home&quot;;	//mapback to home.jsp
	}
</pre><span style="white-space:pre"></span>浏览器中输入: ${url}/view2controller?inString=ABCD回车，Console中显示：XXX<br>
<span style="white-space:pre"></span>如果浏览器中输入: ${url}/view2controller回车，浏览器显示400错误。<br>
<span style="white-space:pre"></span>即：【如果必须传&#20540;，使用@RequestParam要求一下，不必须，直接用view2controller(String reString)】<br>
<p></p>
<p><br>
</p>
<span style="white-space:pre"></span>
<h4>2. 从Controller传&#20540;给视图</h4>
<span style="white-space:pre"></span>在Controller的方法中，创建&quot;Map&lt;String,Object&gt; context&quot;，&quot;context.put(&quot;stringOut&quot;, stringIn)&quot;<br>
<span style="white-space:pre"></span>在hello.jsp中，加入${stringOut}.<br>
<span style="white-space:pre"></span>如：<br>
<span style="white-space:pre"></span>&nbsp; 在HelloController.java文件中，<br>
----------<br>
<pre code_snippet_id="497959" snippet_file_name="blog_20141027_5_2166046"  code_snippet_id="497959" snippet_file_name="blog_20141027_5_2166046" name="code" class="java">	@RequestMapping({&quot;/hello&quot;})
	public String hello(String stringIn，Map&lt;String,Object&gt; context){   //Map String Obj. context
	context.put(&quot;stringOut&quot;, stringIn);	//add context.put&lt;Stringkey, ObjectValue&gt;
	System.out.println(stringIn);
	return &quot;hello&quot;;	
	}</pre><br>
==========<br>
<span style="white-space:pre"></span>&nbsp; 在hello.jsp中：<br>
----------<br>
<pre code_snippet_id="497959" snippet_file_name="blog_20141027_6_3801466"  code_snippet_id="497959" snippet_file_name="blog_20141027_6_3801466" name="code" class="html">	&lt;body&gt;
		&lt;p&gt;hello!!, ${stringOut}.&lt;/p&gt;
	&lt;/body&gt;
</pre>==========
<p></p>
<p><span style="white-space:pre"></span>在浏览器地址栏中输入：{url}/hello?StringIn=my dear redfish<br>
<span style="white-space:pre"></span>显示：hello! my dear red fish.<br>
<br>
</p>
<h4>Spring MVC 传&#20540;(二)——使用Model，从Controller传&#20540;给视图</h4>
<h3><span style="white-space:pre"></span></h3>
<span style="white-space:pre"></span>&nbsp; 在homeController.java文件中，定义method时，使用Model而不是Map<br>
<p>----------</p>
<p><pre code_snippet_id="497959" snippet_file_name="blog_20141124_8_310343"  name="code" class="java">@RequestMapping({&quot;/home&quot;})  
public String home(String homenameIn，Model model){   //我们习惯使用Model而不是Map  
model.addAttribute(&quot;homeMsg&quot;,homenameIn);  //model.addAttribute加入一个分配  
return &quot;home&quot;;    
}  </pre>===========</p>
<span style="white-space:pre"></span>home.jsp中，同样使用<span style="font-family:Consolas,'Courier New',Courier,mono,serif; line-height:18px">${homeMsg}&nbsp;</span>完成显示。
<p></p>
<p><span style="white-space:pre"></span>浏览器中，同样使用 /home?homenameIn= my redfish 完成传&#20540;。<br>
<br>
</p>
<p><br>
</p>
   

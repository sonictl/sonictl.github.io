---
layout: post
title:  "Spring MVC 学习笔记4 ——Controller 与 View 互相传值"
date: 2014-10-28 14:25:37 +0800
tags: 
slug: p20141028142537
---

# Spring MVC 学习笔记4 ——Controller 与 View 互相传值





##### Spring [MVC](https://so.csdn.net/so/search?q=MVC&spm=1001.2101.3001.7020) 传值(一)


###### 1. 从视图传值给Controller


Internal View Resolver 方法, 通过函数的参数来传递。 
  

  
 在HelloController.java文件中， 
  
 ----------- 
  



```
1. @RequestMapping({"/hello"})
2. public String hello(String stringIn){	//此时在浏览器中输入 {url}/hello?stringIn=AAA
3. System.out.println(stringIn);
4. return "hello";
5. }

```
========== 
  


可见，有 public String hello(String stringIn) ，通过函数参数传值给Controller的方法。  
 这里介绍使用@RequestParam来传值给Controller。  
   
 在src/main/java/.../HomeController.java中  
 



```
1. @RequestMapping({"/view2controller"})	// ?inString=AAA,通过这种方式传值、请求
2. public String view2controller(@RequestParam("inString") String reString){
3. //(@RequestParam("inString") String reString)
4. System.out.println(reString);
5. return "home";	//mapback to home.jsp
6. }

```

浏览器中输入: ${url}/view2controller?inString=ABCD回车，Console中显示：XXX 
  

如果浏览器中输入: ${url}/view2controller回车，浏览器显示400错误。 
  

即：【如果必须传值，使用@RequestParam要求一下，不必须，直接用view2controller(String reString)】 
  


  
 



###### 2. 从Controller传值给视图


在Controller的方法中，创建"Map<String,Object> context"，"context.put("stringOut", stringIn)" 
  

在hello.jsp中，加入${stringOut}. 
  

如： 
  

  在HelloController.java文件中， 
  
 ---------- 
  


```
1. @RequestMapping({"/hello"})
2. public String hello(String stringIn，Map<String,Object> context){   //Map String Obj. context
3. context.put("stringOut", stringIn);	//add context.put<Stringkey, ObjectValue>
4. System.out.println(stringIn);
5. return "hello";
6. }

```

  
 ========== 
  

  在hello.jsp中： 
  
 ---------- 
  


```
1. <body>
2. <p>hello!!, ${stringOut}.</p>
3. </body>

```
========== 

在浏览器地址栏中输入：{url}/hello?StringIn=my dear redfish  
 显示：hello! my dear red fish.  
   
 


###### Spring MVC 传值(二)——使用Model，从Controller传值给视图


#### 


  在homeController.java文件中，定义method时，使用Model而不是Map 
  
 ---------- 
  


```
1. @RequestMapping({"/home"})
2. public String home(String homenameIn，Model model){   //我们习惯使用Model而不是Map
3. model.addAttribute("homeMsg",homenameIn);<span style="white-space:pre">	</span>//model.addAttribute加入一个分配
4. return "home";
5. }

```
========== 
  

home.jsp中，同样使用 
${homeMsg} 完成显示。 

浏览器中，同样使用 /home?homenameIn= my redfish 完成传值。  
 





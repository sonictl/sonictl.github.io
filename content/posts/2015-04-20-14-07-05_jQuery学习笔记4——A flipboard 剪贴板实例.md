---
layout: post
title:  "jQuery学习笔记4——A flipboard 剪贴板实例"
date: 2015-04-20 14:07:05 +0800
tags: 
slug: p20150420140705
---

# jQuery学习笔记4——A flipboard 剪贴板实例





 开始之前：jQuery文档(http://docs.jquery.com/) ， js在线IDE(https://jsfiddle.net/)，教程：(http://w3school.com.cn/) 
  

slideDown ——下滑


slideUp —— 向上滑


fadeIn —— 淡入


fadeOut —— 淡出


animate ——动画  
 


        takes two parameters:  
         1. A set of CSS properties,  
         2. A time duration over which to change them.



```
1. //*********关于slideDown和slideUp热身小程序********//
2. //=========点击从上往下出现==========
3. var main = function() {
4. $(".btn").click(function(event) {
5. $(".container").hide();
6. $(".container").slideDown(900);
7. });
8. };
9. 
10. $(document).ready(main);
11. 
12. //=========点击从下往上出现==========
13. 
14. var main = function() {
15. $(".btn").click(function(event) {
16. $(".container").show ();
17. $(".container").slideUp(1100);
18. });
19. };
20. 
21. $(document).ready(main);
22. 
23. //===================================
![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)
```

  

回顾一下next的用法：


用一个经典的click响应滑动显示slides的实例来展示：



```
1. $('.arrow-next').click(function(){//select
2. var currentSlide = $('.active-slide');<span style="white-space:pre">	</span>//declare two boxes, current and next
3. var nextSlide = currentSlide.next(); //the next
4. if (nextSlide.length ===0)//if the nextSlides point to the end
5. nextSlide = $('.slide').first(); //make it point to the first
6. currentSlide.fadeOut(600);<span style="white-space:pre">	</span>//current's actions
7. nextSlide.fadeIn(600);<span style="white-space:pre">		</span>// next's actions
8. currentSlide.removeClass('active-slide');//注意这里没有".", switch the next to current
9. nextSlide.addClass('active-slide'); //switch the next to current.
10. });

```

  


  
 


  
 





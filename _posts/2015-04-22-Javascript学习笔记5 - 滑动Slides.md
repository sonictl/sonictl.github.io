---
layout: post
title: 'Javascript学习笔记5 - 滑动Slides'
date: 2015-04-22 02:12:00 +0800
category: from_cnblogs
slug: p20150422021200
---


<p><span style="color:rgb(102,102,102); font-family:Arial; font-size:14px; line-height:24px; text-indent:28px">开始之前：http://docs.jquery.com/ 是jQuery文档的网站， https://jsfiddle.net/是js的在线验证工具</span><br>
</p>
<p><img src="http://img.blog.csdn.net/20150422100454819?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br>
</p>
<p>在html中，有这几个标签：&nbsp;</p>
<p><img src="http://img.blog.csdn.net/20150422101041717?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br>
</p>
<p><br>
</p>
<p>javascript、jQuery代码：<pre code_snippet_id="649925" snippet_file_name="blog_20150422_1_9860979"  name="code" class="javascript">var main = function(){
    $(&#39;.dropdown-toggle&#39;).click(function(){
        //$(&#39;.dropdown-menu&#39;).slideDown();
        $(&#39;.dropdown-menu&#39;).toggle();
        
    });
    
    // Click Slides -- Show next slide
    $(&#39;.arrow-next&#39;).click(function(){//select
        var currentSlide = $(&#39;.active-slide&#39;);
        var nextSlide = currentSlide.next();
        if (nextSlide.length ===0)
            nextSlide = $(&#39;.slide&#39;).first();
        
        currentSlide.fadeOut(600);
        nextSlide.fadeIn(600);
        currentSlide.removeClass(&#39;active-slide&#39;);//注意这里没有&quot;.&quot;
        nextSlide.addClass(&#39;active-slide&#39;);
        //让点点的颜色跟随变化
        var currentDot = $(&#39;.active-dot&#39;);
        var nextDot = currentDot.next();
        if (nextDot.length === 0)
            nextDot = $(&#39;.dot&#39;).first();
        currentDot.removeClass(&#39;active-dot&#39;);
        nextDot.addClass(&#39;active-dot&#39;);
    });
    
    // Click Slides -- show previous slide
    $(&#39;.arrow-prev&#39;).click(function(){//select
        var currentSlide = $(&#39;.active-slide&#39;);
        var prevSlide = currentSlide.prev();
        if (prevSlide.length === 0)
            prevSlide = $(&#39;.slide&#39;).last();
        
        currentSlide.fadeOut(600);
        prevSlide.fadeIn(600);
        currentSlide.removeClass(&#39;active-slide&#39;);//注意这里没有&quot;.&quot;
        prevSlide.addClass(&#39;active-slide&#39;);
        //让点点的颜色跟随变化
        var currentDot = $(&#39;.active-dot&#39;);
        var prevDot = currentDot.prev();
        if (prevDot.length === 0)
            prevDot = $(&#39;.dot&#39;).last();
        currentDot.removeClass(&#39;active-dot&#39;);
        prevDot.addClass(&#39;active-dot&#39;);
    });
}
    
$(document).ready(main);</pre><br>
</p>
<p>CSS代码：<pre code_snippet_id="649925" snippet_file_name="blog_20150422_2_1350551"  name="code" class="css">/* General */

.container {
  width: 960px;
}


/* Header */

.header {
  background-color: rgba(255, 255, 255, 0.95);
  border-bottom: 1px solid #ddd;
  
  font-family: &#39;Oswald&#39;, sans-serif;
  font-weight: 300;
  
  font-size: 17px;
  padding: 15px;
}


/* Menu */ 

.header .menu {
  float: right;
  list-style: none;
  margin-top: 5px;
}

.menu &gt; li {
  display: inline;
  padding-left: 20px;
  padding-right: 20px;
}

.menu a {
  color: #898989;
}

/* Dropdown */

.dropdown-menu {
  font-size: 16px;
  margin-top: 5px;
  min-width: 105px;
}

.dropdown-menu li a {
  color: #898989;
  padding: 6px 20px;
  font-weight: 300;
}


/* Carousel */

.slider {
  position: relative;
  width: 100%;
  height: 470px;
  border-bottom: 1px solid #ddd;
}

.slide {
  background: transparent url(&#39;http://s3.amazonaws.com/codecademy-content/courses/ltp2/img/flipboard/feature-gradient-transparent.png&#39;) center center no-repeat;
  background-size: cover;
  display: none;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.active-slide {
    display: block;
}

.slide-copy h1 {
  color: #363636;  
  
  font-family: &#39;Oswald&#39;, sans-serif;
  font-weight: 400;
  
  font-size: 40px;
  margin-top: 105px;
  margin-bottom: 0px;
}

.slide-copy h2 {
  color: #b7b7b7;
  
  font-family: &#39;Oswald&#39;, sans-serif;
  font-weight: 400;
  
  font-size: 40px;
  margin: 5px;
}

.slide-copy p {
  color: #959595;
  font-family: Georgia, &quot;Times New Roman&quot;, serif;
  font-size: 1.15em;
  line-height: 1.75em;
  margin-bottom: 15px;
  margin-top: 16px;
}

.slide-img {
  text-align: right;
}

/* Slide feature */

.slide-feature {
  text-align: center;
  background-image: url(&#39;http://s3.amazonaws.com/codecademy-content/courses/ltp2/img/flipboard/ac.png&#39;);
  height: 470px;
}

.slide-feature img {
  margin-top: 112px;
  margin-bottom: 28px;
}

.slide-feature a {
  display: block;
  color: #6fc5e0;
  
  font-family: &quot;HelveticaNeueMdCn&quot;, Helvetica, sans-serif;
  font-family: &#39;Oswald&#39;, sans-serif;
  font-weight: 400;
  
  font-size: 20px;
}

.slider-nav {
  text-align: center;
  margin-top: 20px;
}

.arrow-prev {
  margin-right: 45px;
  display: inline-block;
  vertical-align: top;
  margin-top: 9px;
}

.arrow-next {
  margin-left: 45px;
  display: inline-block;
  vertical-align: top;
  margin-top: 9px;
}

.slider-dots {
  list-style: none;
  display: inline-block;
  padding-left: 0;
  margin-bottom: 0;
}

.slider-dots li {
  color: #bbbcbc;
  display: inline;
  font-size: 30px;
  margin-right: 5px;
}

.slider-dots li.active-dot {
  color: #363636;
}

/* App links */

.get-app {
  list-style: none;
  padding-bottom: 9px;
  padding-left: 0px;
  padding-top: 9px;
}

.get-app li {
  float: left;
  margin-bottom: 5px;
  margin-right: 5px;
}

.get-app img {
  height: 40px;
}</pre><br>
</p>
<p><br>
</p>
   

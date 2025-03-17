---
layout: post
title: '关于封装Dll为Web Service技术方案的讨论'
date: 2014-01-25 03:52:00 +0800
category: from_cnblogs
slug: p20140125035200
---


<p align="center"><strong>关于web架构技术方案的讨论整理</strong></p>
<p align="right">Sonictl 2014年1月25日10:05:52</p>
<p align="right">&nbsp;</p>
<p>本着“三人行必有我师”的学习态度，我在近期跟x老师做了大量沟通，结合我们单位对于“xxx”项目的推进情况，整理一下有关技术方案的讨论结果。</p>
<p>&nbsp;讨论背景：某单位希望把他们在PC上的某算法包DLL封装成WebService服务。</p>
<p><br>
</p>
<p align="left">X老师主张用c&#43;&#43; 来封装web service, 原因如下：</p>
<p align="left">&nbsp;</p>
<p align="left">1、测试 DLL 性能时将外围性能影响降到最低，C&#43;&#43;封的包可以做到这一点。而使用 Ruby/Python/.net 封包以后，Ruby/Python/.net调用DLL的性能会更大程度地影响到整体web service的性能，测试结果会包含Ruby/Python/.net调用DLL的性能和DLL本身的性能，两方面。</p>
<p align="left">&nbsp;</p>
<p align="left">2、C&#43;&#43; 轻量、灵活、可扩展、与 nginx 配合良好。封装出来的接口，完全是单纯的计算任务，与前端的商业业务逻辑毫不相干，架构设计非常容易，几乎零技术成本的就能组装出 1台 WEB 服务器 &#43; 1台数据库服务器 &#43; N 台算法服务器的架构来。</p>
<p align="left">&nbsp;</p>
<p align="left">3、Ruby/Python/.net 调用DLL，确实搭建很快，很容易，可以说没有神马技术含量。ruby或是python，当然是web开发的首选，但我们“xxx”项目是要把一个本地的算法程序转换为web服务。虽然ruby和python都可以调用DLL，但效率都很低。在windows平台下ruby是否靠谱？老师测了一下，单纯构建一个字符串json返回给客户端，他的机器上，ruby方案是500多请求每秒，而c&#43;&#43; rest是2500多每秒，虽然都不是很高，但这么看来，ruby的差距还是挺大的。不过，关于.net，即所说的ashx（不用ashx也可以），调用
 DLL性能上因为是微软自家人，比起其他外来户，的确有它的性能优势，如果我们只是想应付千人左右的同时在线，完全可以就按此技术线路走下去，走不动了再请架构师。</p>
<p align="left">&nbsp;</p>
<p>4、.net 本身可不可以做负载均衡？当然可以，比如：</p>
<p><a target="_blank" target="_blank" href="http://www.cnblogs.com/luminji/archive/2012/05/16/2184280.html"><span style="color:rgb(56,148,193)">http://www.cnblogs.com/luminji/archive/2012/05/16/2184280.html</span></a>，由于x老师更习惯使用Rails，他对.net便不是那么热衷。但他提到，文中的测试数据，<strong>629.93请求/秒</strong>，连1000都没有上，有点低，怀疑IIS性能是否真的有这么差，可能有其他原因。因为根据x老师经验，
 在linux 的 nginx 测试数据，一般都几千上万的。当然，x老师提到完全可以在IIS 前面再装个nginx，但是他在实践中从来没有见到有人这么搭配过。</p>
<p align="left">&nbsp;</p>
<p>5、这个WebService封包的办法很多，进入x老师视线的有 qt webservice（<a target="_blank" target="_blank" href="http://qt-project.org/"><span style="color:#3894C1">http://qt-project.org/</span></a>）（x老师最开始准备采用的方案）、WCF（比较合适）和 ICE（<a target="_blank" target="_blank" href="http://www.zeroc.com/ice.html"><span style="color:#3894C1">http://www.zeroc.com/ice.html</span></a>），其中
 ICE 最牛，号称电信级解决方案。qt 的性价比最高，最后为什么就锁定了c&#43;&#43; webservice了呢？除了上述第2点中说到的轻量、灵活、可扩展、与 nginx 配合良好、服务器架构技术成本低，最主要的原因是，它和 QT 一样都是跨平台的解决方案。c#为什么不推荐使用，因为它锁定windows平台。加之前端若加nginx，除了可做反向代理外，最重要的是它几乎是零成本的负载均衡方案，而nginx在windows下不稳定，一般的生产系统很少这么配。</p>
<p>&nbsp;</p>
<p>6、关于最后锁定C&#43;&#43;，还想补充一点：<span style="color:rgb(51,51,51)">去年，微软开源了代码为</span><span style="color:rgb(51,51,51)"> Casablanca</span><span style="color:rgb(51,51,51)">的</span><span style="color:rgb(51,51,51)">&nbsp;</span><u><a target="_blank" target="_blank" href="http://casablanca.codeplex.com/"><span style="color:rgb(0,105,214)">C&#43;&#43;REST
 SDK</span></a></u><span style="color:rgb(51,51,51)">，目的主要是为了让C&#43;&#43;</span>编程时更加方便的消费 RESTful 服务。但最近它新增了一项功能：<code>New experimental features such asHTTP Listener library</code><span style="color:rgb(51,51,51)">，正是这项功能的出现，我们可以利用 Casablanca</span>，搭建起一个原生代码与云计算服务之间的双向桥梁，轻轻松松的把那些用C、C&#43;&#43;、Delphi
 甚至是 VB 写的单机程序转变成 Web 服务。</p>
<p>&nbsp;</p>
<p>7、关于使用和学习Ruby，x老师给了一个总结：“总结了一下这几天用的技术，http://ruby-china.org/topics/16982&nbsp;，你可以发给你们开发的同学看看。”</p>
<p>&nbsp;</p>
<p><br>
</p>
<p>&nbsp;</p>
   

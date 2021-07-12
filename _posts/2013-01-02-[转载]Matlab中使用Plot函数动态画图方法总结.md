---
layout: post
title: '[转载]Matlab中使用Plot函数动态画图方法总结'
date: 2013-01-02 19:23:00
category: matlab
slug: p20130102192300

---


<div class="articalTitle" style="clear:both; line-height:20px; padding-bottom:10px; color:rgb(70,70,70); font-family:Verdana,宋体,sans-serif; background-color:rgb(188,211,229)">
<h2 id="t_55a4cddc0101aq4w" class="titName SG_txta" style="margin:0px; padding:0px; border:0px; list-style:none; color:rgb(62,115,160); font-size:18px; font-family:微软雅黑,黑体; font-weight:300; display:inline">
[转载]Matlab中使用Plot函数动态画图方法总结</h2>
&nbsp;<span class="time SG_txtc" style="color:rgb(116,116,116); white-space:nowrap; font-family:Arial; font-size:10px; margin-left:5px; margin-right:13px">(2012-12-31 23:26:18)</span><a href="" class="CP_a_fuc" style="text-decoration:initial; color:rgb(62,115,160); margin:0px; font-family:宋体; white-space:nowrap; zoom:1">[<cite style="font-style:normal; font-family:Verdana">删除</cite>]</a>
<div class="turnBoxzz" style="float:right"><a href="" class="SG_aBtn SG_aBtn_ico SG_turn" style="text-decoration:initial; color:rgb(51,51,51); padding:0px 0px 0px 3px; display:inline-block; overflow:hidden; white-space:nowrap; margin-right:6px; position:relative; letter-spacing:5px; width:86px; zoom:1"><cite style="font-style:normal; line-height:23px; padding:0px 20px 0px 32px; height:23px; min-width:1px; overflow-x:visible; width:34px; display:inline-block!important"><img class="SG_icon SG_icon111" src="http://simg.sinajs.cn/blog7style/images/common/sg_trans.gif" width="15" height="15" align="absmiddle" alt="" style="margin:0px; padding:0px; border:0px; list-style:none; position:absolute; left:11px; top:4px">转载<span class="arrow" style="">▼</span></cite></a></div>
</div>
<div class="articalTag" id="sina_keyword_ad_area" style="width:690px; clear:both; word-break:break-all; line-height:20px; color:rgb(70,70,70); font-family:Verdana,宋体,sans-serif; background-color:rgb(188,211,229)">
<table style="margin:0px; padding:0px">
<tbody>
<tr>
<td class="blog_tag" style="margin:0px; padding:0px 10px 0px 0px; font-family:宋体; vertical-align:top">
<span class="SG_txtb" style="color:rgb(70,70,70)">标签：</span>&nbsp;
<h3 style="margin:0px 5px 0px 0px; padding:0px; border:0px; list-style:none; display:inline; font-size:12px; font-weight:normal">
<a href="http://search.sina.com.cn/?c=blog&amp;q=%D7%AA%D4%D8&amp;by=tag" target="_blank" style="text-decoration:initial; color:rgb(62,115,160); white-space:nowrap">转载</a></h3>
</td>
<td class="blog_class" style="margin:0px; padding:0px; font-family:宋体; vertical-align:top; width:220px; white-space:nowrap">
<span class="SG_txtb" style="color:rgb(70,70,70)">分类：</span>&nbsp;<a target="_blank" href="http://blog.sina.com.cn/s/articlelist_1436863964_1_1.html" style="text-decoration:initial; color:rgb(62,115,160)">技术相关</a></td>
</tr>
</tbody>
</table>
</div>
<div id="sina_keyword_ad_area2" class="articalContent  " style="width:690px; clear:both; padding-top:18px; font-size:14px; line-height:21px; padding-bottom:30px; word-wrap:normal; word-break:normal; overflow:hidden; font-family:simsun; color:rgb(70,70,70); background-color:rgb(188,211,229)">
<div class="blogzz_abstract borderc" style="border-top-width:1px; border-style:dotted none none; border-top-color:rgb(204,204,204); padding-top:15px; margin:20px 0px">
<div class="blogzz_ainfo" style="margin-bottom:12px"><span style="word-wrap:normal; word-break:normal; margin-right:25px"><strong>原文地址：</strong><a target="_blank" href="http://blog.sina.com.cn/s/blog_49d955150100mfnn.html" title="Matlab中使用Plot函数动态画图方法总结" style="text-decoration:initial; color:rgb(62,115,160)">Matlab中使用Plot函数动态画图方法总结</a></span><span style="word-wrap:normal; word-break:normal"><strong>作者：</strong><a href="http://blog.sina.com.cn/u/1238979861" title="gypsy" target="_blank" style="text-decoration:initial; color:rgb(62,115,160)">gypsy</a></span></div>
<div class="blogzz_acon">
<p align="center" style="margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal">
Matlab中使用Plot函数动态画图方法总结</p>
<div>
<table cellspacing="0" cellpadding="0" style="margin:0px; padding:0px">
<tbody>
<tr>
<td style="margin:0px; padding:0px; font-family:Verdana,宋体,sans-serif; line-height:18px">
Matlab除了强大的矩阵运算，仿真分析外，绘图功能也是相当的强大，静态画图没什么问题，由于Matlab本身的多线程编程缺陷，想要动态的画图，并且能够很好的在GUI中得到控制，还不是一件很容易的事情，下面总结几种方法。<br>
<br>
<span style="color:red; word-wrap:normal; word-break:normal"><strong>一. AXIS 移动坐标系</strong></span><br>
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;这种方法是最简单的一种方法，适合于数据已经全部生成的场合，先画图，然后移动坐标轴。实例代码如下：<br>
<br>
<div>
<ol style="margin:0px; padding:0px; border:0px; list-style:none">
<li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
%%<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
%先画好，然后更改坐标系<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
%在命令行中 使用 Ctrl&#43;C 结束<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
t=0:0.1:100*pi;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
m=sin(t);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
plot(t,m);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
x=-2*pi;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
axis([x,x&#43;4*pi,-2,2]);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
grid on<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
while 1<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
if x&gt;max(t)<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
break;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
end<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
x=x&#43;0.1;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
axis([x,x&#43;4*pi,-2,2]); %移动坐标系<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
pause(0.1);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
end</li></ol>
</div>
<strong><span style="color:red; word-wrap:normal; word-break:normal">二. Hold On 模式</span></strong><br>
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;<wbr>&nbsp;<wbr><br>
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;此种方法比较原始，适合于即时数据，原理是先画上一帧，接着保留原始图像，追加下一幀图像，此种方式比较繁琐，涉及画图细节，并且没有完整并连续的Line对象数据。<br>
<br>
&nbsp;<wbr>&nbsp;&nbsp;<wbr>例如：<br>
<br>
<div>
<ol style="margin:0px; padding:0px; border:0px; list-style:none">
<li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
%%<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
% Hold On 法<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
% 此种方法只能点，或者分段划线<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
hold off<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
t=0;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
m=0;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
t1=[0 0.1]; %要构成序列<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
m1=[sin(t1);cos(t1)];<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
p = plot(t,m,'*',t1,m1(1,:),'-r',t1,m1(2,:),'-b','MarkerSize',5);&nbsp;<wbr>&nbsp;&nbsp;<wbr><br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
x=-1.5*pi;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
axis([x x&#43;2*pi -1.5 1.5]);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
grid on;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
for i=1:100<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;hold on<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;t=0.1*i; %下一个点<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;m=t-floor(t);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;t1=t1&#43;0.1; %下一段线(组)<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;m1=[sin(t1);cos(t1)];<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;p = plot(t,m,'*',t1,m1(1,:),'-r',t1,m1(2,:),'-b','MarkerSize',5);&nbsp;<wbr>&nbsp;&nbsp;<wbr><br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;x=x&#43;0.1;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;axis([x x&#43;2*pi -1.5 1.5]);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;pause(0.01);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
end</li></ol>
</div>
<strong><span style="color:red; word-wrap:normal; word-break:normal">三. Plot 背景擦除模式</span></strong><br>
<br>
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;<wbr>&nbsp;&nbsp;<wbr>这种模式比较适合画动画，效率比较高，刷新闪烁小，适合即时数据，最终的Line结构数据完整。<br>
<br>
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;<wbr>&nbsp;&nbsp;<wbr>了解此方法之前要搞清楚 Plot函数的原型是什么： Plot函数，输入为 X-Y (-X)坐标元组、以及“属性”-“&#20540;对，输出为一个列向量（每条曲线岁对应的Line结构 Handle，每一行代表一个 线条的handles）, 每一线条都有 XData，YData 向量。如果你画了2条线，那么会返回 2×1的向量。<br>
重新画图不需要 重新书写 Plot，只需要 刷新图像即可，使用drawnow函数。<br>
<br>
完整实例如下：<br>
<br>
<span style="color:blue; word-wrap:normal; word-break:normal">1. 画一个点的动画：<br>
</span><br>
<div>
<ol style="margin:0px; padding:0px; border:0px; list-style:none">
<li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
%%<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
%采用背景擦除的方法，动态的划点，并且动态改变坐标系<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
% t,m 均为一行 ，并且不能为多行<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
t=0;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
m=0;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
p = plot(t,m,'*',...<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>'EraseMode','background','MarkerSize',5);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
x=-1.5*pi;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
axis([x x&#43;2*pi -1.5 1.5]);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
grid on;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
for i=1:1000<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;t=0.1*i;&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;%两个变量均不追加<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;m=sin(0.1*i);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;set(p,'XData',t,'YData',m)<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;x=x&#43;0.1;&nbsp;<wbr>&nbsp;&nbsp;<wbr><br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;drawnow<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;axis([x x&#43;2*pi -1.5 1.5]);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;pause(0.1);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
end</li></ol>
</div>
<div>&nbsp;<wbr></div>
<div><span style="color:blue; word-wrap:normal; word-break:normal">2. 动态多条曲线(即时数据)</span></div>
<div>
<ol style="margin:0px; padding:0px; border:0px; list-style:none">
<li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
%%<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
%采用背景擦除的方法，动态的划线，并且动态改变坐标系<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
% 多行划线<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
t=[0]<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
m=[sin(t);cos(t)]<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
p = plot(t,m,...<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>'EraseMode','background','MarkerSize',5);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
x=-1.5*pi;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
axis([x x&#43;2*pi -1.5 1.5]);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
grid on;<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
for i=1:1000<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;t=[t 0.1*i];&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;%Matrix 1*(i&#43;1)<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;m=[m [sin(0.1*i);cos(0.1*i)]]; %Matrix 2*(i&#43;1)<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;set(p(1),'XData',t,'YData',m(1,:))<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;set(p(2),'XData',t,'YData',m(2,:))&nbsp;<wbr>&nbsp;&nbsp;<wbr><br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;drawnow<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;x=x&#43;0.1;&nbsp;<wbr>&nbsp;&nbsp;<wbr><br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;axis([x x&#43;2*pi -1.5 1.5]);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
&nbsp;<wbr>&nbsp;&nbsp;<wbr>&nbsp;pause(0.5);<br>
</li><li style="margin:0px 0px 0px 30px; padding:0px; border:0px; list-style:decimal">
end</li></ol>
</div>
<p style="margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal">
&nbsp;<wbr>&nbsp;<wbr>&nbsp;<wbr>&nbsp;<wbr>&nbsp;<wbr>&nbsp;<wbr>上面的这几个画图方式的示例只是简单的for循环，是单线程的，如果是涉及到GUI的编程，那么请使用Timer来完成这件事情，Timer是我在Matlab中实现多线程唯一方法(没有找到别的方法)。</p>
<p style="margin-top:0px; margin-bottom:5px; padding-top:0px; padding-bottom:0px; border:0px; list-style:none; word-wrap:normal; word-break:normal">
来源：<a href="http://www.matlabfan.com/thread-736-1-6.html" style="text-decoration:initial; color:rgb(62,115,160)">http://www.matlabfan.com/thread-736-1-6.html</a></p>
</td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
</div>
   

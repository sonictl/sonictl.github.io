---
layout: post
title: '使用Gnu gprof进行Linux平台下的程序分析'
date: 2013-01-20 15:22:00 +0800
category: from_cnblogs
slug: p20130120152200
---


http://www.pcdog.com/edu/linux/18/10/u231314_1.html<br>
http://www.pcdog.com/edu/linux/18/10/u231314_2.html<br>
http://www.pcdog.com/edu/linux/18/10/u231314_3.html<br>
<br>
<br>
1. Compile source .c code for analysis, Create executable file:<br>
<pre>&nbsp; &nbsp; &nbsp; &nbsp; <span style="font-family:Courier New">&gt;&gt; gcc -pg test.c -o test</span>&nbsp;</pre>
<br>
2. Launch executable file, Create gmon.out<br>
<pre>&nbsp; &nbsp; &nbsp; &nbsp;<span style="font-family:Courier New"> &gt;&gt; ./test</span></pre>
<br>
3. Use gmon.out to analysis:<br>
<pre>&nbsp; &nbsp; &nbsp; &nbsp; &gt;&gt; &nbsp;gprof -b test gmon.out | less</pre>
<pre>&nbsp; &nbsp; or: &gt;&gt; &nbsp;gprof -b test gmon.out | more</pre>
<br>
4.&nbsp;<br>
&nbsp; &nbsp;&nbsp;<br>
&nbsp; &nbsp; &nbsp;<img src="http://img.my.csdn.net/uploads/201301/20/1358695524_8629.png" alt=""><br>
<br>
常用的Gprof 命令选项解释：<br>
&nbsp;<br>
-b不再输出统计图表中每个字段的详细描述。<br>
<br>
-p 只输出函数的调用图（Call graph 的那部分信息）。<br>
<br>
-q 只输出函数的时间消耗列表。<br>
<br>
-E Name不再输出函数Name 及其子函数的调用图，此标志类&#20284;于 -e 标志，但它在总时间和百分比时间的计算中排除了由函数Name 及其子函数所用的时间。<br>
<br>
-e Name 不再输出函数Name 及其子函数的调用图（除非它们有未被限制的其它父函数）。可以给定多个 -e 标志。一个 -e 标志只能指定一个函数。<br>
<br>
-F Name 输出函数Name 及其子函数的调用图，它类&#20284;于 -f 标志，但它在总时间和百分比时间计算中仅使用所打印的例程的时间。可以指定多个 -F 标志。一个 -F 标志只能指定一个函数。-F 标志覆盖 -E 标志。<br>
<br>
-f Name输出函数Name 及其子函数的调用图。可以指定多个 -f 标志。一个 -f 标志只能指定一个函数。<br>
<br>
-z 显示使用次数为零的例程（按照调用计数和累积时间计算）。<br>
<br>
<p>English Reference:<a href="http://www.cs.utah.edu/dept/old/texinfo/as/gprof.html#SEC4" target="_blank">http://www.cs.utah.edu/dept/old/texinfo/as/gprof.html#SEC4</a></p>
<p><br>
</p>
<p>--------附： vi使用方法：---------<br>
<br>
<br>
　　vi 的三种命令模式　　<br>
<br>
<br>
Command（命令）模式，用于输入命令<br>
　　Insert（插入）模式，用于插入文本<br>
　　Visual（可视）模式，用于视化的的高亮并选定正文<br>
　　光标移动　　<br>
　　当我们按ESC进入Command模式后，我们可以用下面的一些键位来移动光标；<br>
　　j 向下移动一行<br>
　　k 向上移动一行<br>
　　h 向左移动一个字符<br>
　　l 向右移动一个字符<br>
　　ctrl&#43;b 向上移动一屏<br>
　　ctrl&#43;f 向下移动一屏<br>
　　向上箭头 向上移动<br>
　　向下箭头 向下移动<br>
　　向左箭头 向左移动<br>
　　向右箭头 向右移动<br>
　　我们编辑一个文件时，对于 j、k、l和h键，还能在这些动作命令的前面加上数字，比如 3j，表示向下移动3行。<br>
　　/# &#43;Enter #为查找的内容　<br>
　　插入模式（文本的插入）<br>
　　<br>
　　i 在光标之前插入<br>
　　a 在光标之后插入<br>
　　I 在光标所在行的行首插入<br>
　　A 在光标所在行的行末插入<br>
　　o 在光标所在的行的下面插入一行<br>
　　O 在光标所在的行的上面插入一行<br>
　　s 用输入的文本替换光标所在字符<br>
　　S 用输入的文本替换光标所在行　<br>
　　文本内容的删除操作；<br>
　　<br>
　　x 一个字符<br>
　　#x 删除几个字符，#表示数字，比如3x<br>
　　dw 删除一个单词<br>
　　#dw 删除几个单词，#用数字表示，比如3dw表示删除三个单词<br>
　　dd 删除一行；<br>
　　#dd 删除多个行，#代表数字，比如3dd 表示删除光标行及光标的下两行<br>
　　d$ 删除光标到行尾的内容<br>
　　J 清除光标所处的行与下一行之间的换行，行尾没有空&#26684;的话会自动添加一个空&#26684;。<br>
　　#J 表示合并#(数字)行。<br>
　　退出保存；<br>
　　在命令模式下按 shift&#43;: 文本底端出现冒号<br>
　　:w 保存；<br>
　　:w filename 另存为filename；<br>
　　:wq! 保存退出；<br>
　　:wq! filename 注：以filename为文件名保存后退出；<br>
　　:q! 不保存退出；<br>
　　:x 应该是保存并退出 ，功能和:wq!相同<br>
　　撤销操作<br>
　　u命令取消最近一次的操作，可以使用多次来恢复原有的操作[1]<br>
　　U取消所有操作<br>
　　Ctrl&#43;R可以恢复对使用u命令的操作<br>
　　复制操作<br>
　　yy命令复制当前整行的内容到vi缓冲区<br>
　　yw复制当前光标所在位置到单词尾字符的内容到vi缓存区，相当于复制一个单词<br>
　　y$复制光标所在位置到行尾内容到缓存区<br>
　　y^复制光标所在位置到行首内容到缓存区<br>
　　#yy例如：5yy就是复制5行<br>
　　#yw例如：2yw就是复制两个单词<br>
　　如果要复制第m行到第n行之间的内容，可以在末行模式中输入m，ny例如：3，5y复制第三行到第五行内容到缓存区。<br>
　　查找和替换<br>
　　vi的查找和替换功能主要在末行模式完成：<br>
　　至上而下的查找<br>
　　/ 要查找的字符串，其中/代表从光标所在位置起开始查找，例如：/ work<br>
　　至下而上的查找<br>
　　?要查找的字符串 例如：? work<br>
　　替换<br>
　　:s/old/new用new替换行中首次出现的old<br>
　　: s/old/new/g 用new替换行中所有出现的old<br>
　　:#,# s/old/new/g用new替换从第#行到第#行中出现的old<br>
　　:% s/old/new/g用new替换整篇中出现的old<br>
　　如果替换的范围较大时，在所有的命令尾加一个c命令，强制每个替换需要用户进行确认，例如:s/old/new/c 或s/old/new/gc<br>
　　恢复文件　　<br>
　vi在编辑某一个文件时，会生成一个临时文件，这个文件以 . 开头并以 .swp结尾。正常退出该文件自动删除，如果意外退出例如忽然断电，该文件不会删除，我们在下次编辑时可以选择一下命令处理：<br>
　　O只读打开，不改变文件内容<br>
　　E继续编辑文件，不恢复.swp文件保存的内容<br>
　　R将恢复上次编辑以后未保存文件内容<br>
　　Q退出vi<br>
　　D删除.swp文件<br>
　　或者使用vi －r 文件名来恢复未保存的内容<br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
   

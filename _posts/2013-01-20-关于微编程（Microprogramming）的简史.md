---
layout: post
title: '关于微编程（Microprogramming）的简史'
date: 2013-01-20 15:01:00 +0800
category: from_cnblogs
slug: p20130120150100
---


<div style="color:rgb(70,70,70); font-family:simsun; font-size:14px; line-height:21px">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
A brief history of Microprogramming<br>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
关于微编程（Microprogramming）的简史</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
&nbsp;<a href="http://www.cs.clemson.edu/~mark/uprog.html">http://www.cs.clemson.edu/~mark/uprog.html</a>&nbsp; &lt;--- the source text of this translation.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
Mark Smotherman</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
总论： 为了在处理器内执行指令，需要一些控制逻辑，微编程就是实现这些控制逻辑的技术。它建立在两项操作上，一是从控制库取出底层的微指令。二是从每条微指令中提取合适的控制信号和微程序序列信息。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
-----------------------------------</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
定义和示例</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
尽管有些零星的用法将“微编程”和“编程一个微机”等同起来，但这是不标准的定义。反之，微编程是一个系统级的实现计算机中央处理单元的控制逻辑的技术。它是一种以贮存程序逻辑的形式，用以替代硬件控制电路。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
一个计算机的中央控制单元由数据路径和控制单元组成。 数据路径包括：寄存器，运算功能单元（functional unit，如移位器，ALU等），内部处理器总线和路径，主存接口单元和I/O总线。 控制单元负责一系列的步骤，这些步骤是用户可见的指令的执行期间，数据路径采取的步骤。那些指令或许是“宏指令”，比如：load, add, store等等。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
数据路径的每一个动作都被称为“寄存器转移”，它们都牵涉到信息在数据路径之间的转移，也许是运算功能单元在进行数据的转移，地址的转移，或者是指令位的转移。一次寄存器转移的完成可能由以下步骤构成：从寄存器发出内容到内部处理器总线，选择ALU的操作，移位等等。通过这样的步骤，信息会传输，在一个或多个寄存器的间进行发送和接收新&#20540;。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
寄存器使能的信号控制了对寄存器发收数据的操作。操作选择信号控制了运算功能单元进行何种运算。他们都被称作控制信号，被控制单元所支持。一堆逻辑门电路使得数据路径对信号的传输信号的响应、在寄存器之间读写数据成为可能。他们被成为“控制点”。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
一条宏指令的执行期间的每一步都由很多动作，比如寄存器传输，还需要按时间高度合理排列的一系列控制信号。一个单独的数据路径的动作通常被称为“微操作” microoperations.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
未完不想续了。。。微编程是早期计算机使用的方法，现在已经不用，如果遇到有些老师要求写点introduce of micrioprogramming, you can just study the content in the link below. Good Luck~</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
原文地址：&nbsp;<a href="http://www.cs.clemson.edu/~mark/uprog.html">http://www.cs.clemson.edu/~mark/uprog.html</a></div>
<br>
A brief history of Microprogramming<br>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
关于微编程（Microprogramming）的简史</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
&nbsp;<a href="http://www.cs.clemson.edu/~mark/uprog.html">http://www.cs.clemson.edu/~mark/uprog.html</a>&nbsp; &lt;--- the source text of this translation.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
Mark Smotherman</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
总论： 为了在处理器内执行指令，需要一些控制逻辑，微编程就是实现这些控制逻辑的技术。它建立在两项操作上，一是从控制库取出底层的微指令。二是从每条微指令中提取合适的控制信号和微程序序列信息。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
-----------------------------------</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
定义和示例</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
尽管有些零星的用法将“微编程”和“编程一个微机”等同起来，但这是不标准的定义。反之，微编程是一个系统级的实现计算机中央处理单元的控制逻辑的技术。它是一种以贮存程序逻辑的形式，用以替代硬件控制电路。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
一个计算机的中央控制单元由数据路径和控制单元组成。 数据路径包括：寄存器，运算功能单元（functional unit，如移位器，ALU等），内部处理器总线和路径，主存接口单元和I/O总线。 控制单元负责一系列的步骤，这些步骤是用户可见的指令的执行期间，数据路径采取的步骤。那些指令或许是“宏指令”，比如：load, add, store等等。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
数据路径的每一个动作都被称为“寄存器转移”，它们都牵涉到信息在数据路径之间的转移，也许是运算功能单元在进行数据的转移，地址的转移，或者是指令位的转移。一次寄存器转移的完成可能由以下步骤构成：从寄存器发出内容到内部处理器总线，选择ALU的操作，移位等等。通过这样的步骤，信息会传输，在一个或多个寄存器的间进行发送和接收新&#20540;。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
寄存器使能的信号控制了对寄存器发收数据的操作。操作选择信号控制了运算功能单元进行何种运算。他们都被称作控制信号，被控制单元所支持。一堆逻辑门电路使得数据路径对信号的传输信号的响应、在寄存器之间读写数据成为可能。他们被成为“控制点”。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
一条宏指令的执行期间的每一步都由很多动作，比如寄存器传输，还需要按时间高度合理排列的一系列控制信号。一个单独的数据路径的动作通常被称为“微操作” microoperations.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
未完不想续了。。。微编程是早期计算机使用的方法，现在已经不用，如果遇到有些老师要求写点introduce of micrioprogramming, you can just study the content in the link below. Good Luck~</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
原文地址：&nbsp;<a href="http://www.cs.clemson.edu/~mark/uprog.html">http://www.cs.clemson.edu/~mark/uprog.html</a></div>
<br>
A brief history of Microprogramming<br>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
关于微编程（Microprogramming）的简史</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
&nbsp;<a href="http://www.cs.clemson.edu/~mark/uprog.html">http://www.cs.clemson.edu/~mark/uprog.html</a>&nbsp; &lt;--- the source text of this translation.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
Mark Smotherman</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
总论： 为了在处理器内执行指令，需要一些控制逻辑，微编程就是实现这些控制逻辑的技术。它建立在两项操作上，一是从控制库取出底层的微指令。二是从每条微指令中提取合适的控制信号和微程序序列信息。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
-----------------------------------</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
定义和示例</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
尽管有些零星的用法将“微编程”和“编程一个微机”等同起来，但这是不标准的定义。反之，微编程是一个系统级的实现计算机中央处理单元的控制逻辑的技术。它是一种以贮存程序逻辑的形式，用以替代硬件控制电路。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
一个计算机的中央控制单元由数据路径和控制单元组成。 数据路径包括：寄存器，运算功能单元（functional unit，如移位器，ALU等），内部处理器总线和路径，主存接口单元和I/O总线。 控制单元负责一系列的步骤，这些步骤是用户可见的指令的执行期间，数据路径采取的步骤。那些指令或许是“宏指令”，比如：load, add, store等等。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
数据路径的每一个动作都被称为“寄存器转移”，它们都牵涉到信息在数据路径之间的转移，也许是运算功能单元在进行数据的转移，地址的转移，或者是指令位的转移。一次寄存器转移的完成可能由以下步骤构成：从寄存器发出内容到内部处理器总线，选择ALU的操作，移位等等。通过这样的步骤，信息会传输，在一个或多个寄存器的间进行发送和接收新&#20540;。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
寄存器使能的信号控制了对寄存器发收数据的操作。操作选择信号控制了运算功能单元进行何种运算。他们都被称作控制信号，被控制单元所支持。一堆逻辑门电路使得数据路径对信号的传输信号的响应、在寄存器之间读写数据成为可能。他们被成为“控制点”。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
一条宏指令的执行期间的每一步都由很多动作，比如寄存器传输，还需要按时间高度合理排列的一系列控制信号。一个单独的数据路径的动作通常被称为“微操作” microoperations.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
未完不想续了。。。微编程是早期计算机使用的方法，现在已经不用，如果遇到有些老师要求写点introduce of micrioprogramming, you can just study the content in the link below. Good Luck~</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
原文地址：&nbsp;<a href="http://www.cs.clemson.edu/~mark/uprog.html">http://www.cs.clemson.edu/~mark/uprog.html</a></div>
<br>
A brief history of Microprogramming<br>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
关于微编程（Microprogramming）的简史</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
&nbsp;<a href="http://www.cs.clemson.edu/~mark/uprog.html">http://www.cs.clemson.edu/~mark/uprog.html</a>&nbsp; &lt;--- the source text of this translation.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
Mark Smotherman</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
总论： 为了在处理器内执行指令，需要一些控制逻辑，微编程就是实现这些控制逻辑的技术。它建立在两项操作上，一是从控制库取出底层的微指令。二是从每条微指令中提取合适的控制信号和微程序序列信息。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
-----------------------------------</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
定义和示例</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
尽管有些零星的用法将“微编程”和“编程一个微机”等同起来，但这是不标准的定义。反之，微编程是一个系统级的实现计算机中央处理单元的控制逻辑的技术。它是一种以贮存程序逻辑的形式，用以替代硬件控制电路。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
一个计算机的中央控制单元由数据路径和控制单元组成。 数据路径包括：寄存器，运算功能单元（functional unit，如移位器，ALU等），内部处理器总线和路径，主存接口单元和I/O总线。 控制单元负责一系列的步骤，这些步骤是用户可见的指令的执行期间，数据路径采取的步骤。那些指令或许是“宏指令”，比如：load, add, store等等。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
数据路径的每一个动作都被称为“寄存器转移”，它们都牵涉到信息在数据路径之间的转移，也许是运算功能单元在进行数据的转移，地址的转移，或者是指令位的转移。一次寄存器转移的完成可能由以下步骤构成：从寄存器发出内容到内部处理器总线，选择ALU的操作，移位等等。通过这样的步骤，信息会传输，在一个或多个寄存器的间进行发送和接收新&#20540;。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
寄存器使能的信号控制了对寄存器发收数据的操作。操作选择信号控制了运算功能单元进行何种运算。他们都被称作控制信号，被控制单元所支持。一堆逻辑门电路使得数据路径对信号的传输信号的响应、在寄存器之间读写数据成为可能。他们被成为“控制点”。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
一条宏指令的执行期间的每一步都由很多动作，比如寄存器传输，还需要按时间高度合理排列的一系列控制信号。一个单独的数据路径的动作通常被称为“微操作” microoperations.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
未完不想续了。。。微编程是早期计算机使用的方法，现在已经不用，如果遇到有些老师要求写点introduce of micrioprogramming, you can just study the content in the link below. Good Luck~</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
原文地址：&nbsp;<a href="http://www.cs.clemson.edu/~mark/uprog.html">http://www.cs.clemson.edu/~mark/uprog.html</a></div>
<div style="color:rgb(70,70,70); font-family:simsun; font-size:14px; line-height:21px; background-color:rgb(188,211,229)">
<a href="http://www.cs.clemson.edu/~mark/uprog.html" style="text-decoration:initial; color:rgb(62,115,160)"></a></div>
<div style="top:0px">
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
关于微编程（Microprogramming）的简史</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
&nbsp;<a href="http://www.cs.clemson.edu/~mark/uprog.html">http://www.cs.clemson.edu/~mark/uprog.html</a>&nbsp; &lt;--- the source text of this translation.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
Mark Smotherman</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
总论： 为了在处理器内执行指令，需要一些控制逻辑，微编程就是实现这些控制逻辑的技术。它建立在两项操作上，一是从控制库取出底层的微指令。二是从每条微指令中提取合适的控制信号和微程序序列信息。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
-----------------------------------</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
定义和示例</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
尽管有些零星的用法将“微编程”和“编程一个微机”等同起来，但这是不标准的定义。反之，微编程是一个系统级的实现计算机中央处理单元的控制逻辑的技术。它是一种以贮存程序逻辑的形式，用以替代硬件控制电路。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
一个计算机的中央控制单元由数据路径和控制单元组成。 数据路径包括：寄存器，运算功能单元（functional unit，如移位器，ALU等），内部处理器总线和路径，主存接口单元和I/O总线。 控制单元负责一系列的步骤，这些步骤是用户可见的指令的执行期间，数据路径采取的步骤。那些指令或许是“宏指令”，比如：load, add, store等等。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
数据路径的每一个动作都被称为“寄存器转移”，它们都牵涉到信息在数据路径之间的转移，也许是运算功能单元在进行数据的转移，地址的转移，或者是指令位的转移。一次寄存器转移的完成可能由以下步骤构成：从寄存器发出内容到内部处理器总线，选择ALU的操作，移位等等。通过这样的步骤，信息会传输，在一个或多个寄存器的间进行发送和接收新&#20540;。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
寄存器使能的信号控制了对寄存器发收数据的操作。操作选择信号控制了运算功能单元进行何种运算。他们都被称作控制信号，被控制单元所支持。一堆逻辑门电路使得数据路径对信号的传输信号的响应、在寄存器之间读写数据成为可能。他们被成为“控制点”。</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
一条宏指令的执行期间的每一步都由很多动作，比如寄存器传输，还需要按时间高度合理排列的一系列控制信号。一个单独的数据路径的动作通常被称为“微操作” microoperations.</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
未完不想续了。。。微编程是早期计算机使用的方法，现在已经不用，如果遇到有些老师要求写点introduce of micrioprogramming, you can just study the content in the link below. Good Luck~</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
原文地址：&nbsp;<a href="http://www.cs.clemson.edu/~mark/uprog.html">http://www.cs.clemson.edu/~mark/uprog.html</a></div>
</div>
   

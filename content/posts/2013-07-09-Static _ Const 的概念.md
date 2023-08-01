---
layout: post
title: 'Static / Const 的概念'
date: 2013-07-09 02:56:00 +0800
category: from_cnblogs
slug: p20130709025600
---


<p>C/C&#43;&#43;/Java Static / Const 的概念<br>
这里以C为准，其他语言类&#20284;。<br>
Static变量是指分配不变（只可分配一次，以后再分配就无效了。）的变量 -- 它的存活寿命或伸展域可以贯穿程序运行的所有过程。这个概念与“ephemeral-短命的”，分配即变的，变量恰恰相反。常常被人们称作的局部变量就是分配即变的。分配即变的变量的存储空间的分配或者回收都是在Stack上完成。相反，分配不变变量的存储空间实在heap memory上动态分配的。<br>
当一个程序被加载到内存，Static变量被存放在程序的地址空间的数据区（如果已初始化了），或者BBS区（BBS Segment）（如果没有被初始化）。 并且在加载之前就被存放在对应的对象文件的区域。<br>
Static 关键字在 C 语言和其他相关语言中都是指这个静态分配的意思或类&#20284;意思。<br>
<br>
--- 作用域 ---<br>
Scope[edit]<br>
See also: Variable (computer science)#Scope and extent<br>
In terms of scope and extent, static variables have extent the entire run of the program, but may have more limited scope. A basic distinction is between a static global variable, which is in scope throughout the program, and a static local variable, which
 is only in scope within a function (or other local scope). A static variable may also have module scope or some variant, such as internal linkage in C, which is a form of file scope or module scope.<br>
In object-oriented programming there is also the concept of a static member variable, which is a &quot;class variable&quot; of a statically defined class – a member variable of a given class which is shared across all instances (objects), and is accessible as a member
 variable of these objects. Note however that a class variable of a dynamically defined class, in languages where classes can be defined at run time, is allocated when the class is defined and is not static.<br>
&nbsp;--- 程序示例 Example ---<br>
An example of static local variable in C:</p>
<p><pre name="code" class="cpp">#include &lt;stdio.h&gt;
 
void func() {
        static int x = 0; // x is initialized only once across three calls of func()
        printf(&quot;%d\n&quot;, x); // outputs the value of x
        x = x + 1;
}
 
int main(int argc, char *argv[]) {
        func(); // prints 0
        func(); // prints 1
        func(); // prints 2
        return 0;
}</pre><br>
<br>
</p>
   

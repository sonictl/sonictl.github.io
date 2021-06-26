---
layout: post
title:  "python数组和矩阵使用总结"
date:   2017-08-21 20:41:34 +0800
categories: jekyll update
---
#   python数组和矩阵使用总结
](http://blog.csdn.net/everlasting_188/article/details/53484290)

##  1、数组和矩阵常见用法

>   * [ Python ](http://lib.csdn.net/base/python "Python知识库") 使用NumPy包完成了对N-
> 维数组的快速便捷操作。使用这个包，需要导入numpy。
>   *
> SciPy包以NumPy包为基础，大大的扩展了numpy的能力。因此只要导入了scipy，不必在单独导入numpy了！为了使用的方便，scipy包在最外层名字空间中包括了所有的numpy内容。
>   * 本文还是区分numpy中实现的和scipy中实现的。
>   * 以下默认已经：import numpy as np 以及 impor scipy as sp

numpy的基本类型是 **多维数组** ，把matrix看做是array的子类。

##  **1.建立矩阵**

a1=np.array([1,2,3],dtype=int)
## 建立一个一维数组，数据类型是int。也可以不指定数据类型，使用默认。几乎所有的数组建立函数都可以指定数据类型，即dtype的取值。

a2=np.array([[1,2,3],[2,3,4]]) #建立一个二维数组。此处和MATLAB的二维数组（矩阵）的建立有很大差别。

同样，numpy中也有很多内置的特殊矩阵：

b1=np.zeros((2,3))
## 生成一个2行3列的全0矩阵。注意，参数是一个tuple：(2,3)，所以有两个括号。完整的形式为：zeros(shape,dtype=)。相同的结构，有
**ones()** 建立全1矩阵。 **empty()** 建立一个空矩阵，使用内存中的随机值来填充这个矩阵。

###  Numpy将二维数组添加到空数组: numpy 初始化空数组，动态增加数据点


​    
    a=np.empty([0,3])
    b = np.array([[1,2,3],[4,5,6]])
    c=[[7,8,9]]
     
    print(a.shape, 'a shape')
    print(b.shape, 'b shape')


​     
    a = np.append(a, b, axis=0)
    a = np.append(a, c, axis=0)
     
    print(a.shape, 'a append b')
    print(a.shape, 'a append b append c')


b2=identity(n) #建立n*n的单位阵，这只能是一个方阵。

b3=eye(N,M=None,k=0) #建立一个对角线是1其余值为0的矩阵，用k指定对角线的位置。M默认None。

此外，numpy中还提供了几个like函数，即按照某一个已知的数组的规模（几行几列）建立同样规模的特殊数组。这样的函数有
**zeros_like()、empty_like()、ones_like()，**
它们的参数均为如此形式：zeros_like(a,dtype=)，其中，a是一个已知的数组。

c1=np.arange(2,3,0.1) #起点，终点，步长值。含起点值，不含终点值。

c2=np.linspace(1,4,10) #起点，终点，区间内点数。起点终点均包括在内。同理，有logspace()函数

d1=np.linalg.companion(a) #伴随矩阵

d2=np.linalg.triu()/tril() #作用同MATLAB中的同名函数

e1=np.random.rand(3,2) #产生一个3行2列的随机数组。同一空间下，有randn()/randint()等多个随机函数

fliplr()/flipud()/rot90() #功能类似MATLAB同名函数。

xx=np.roll(x,2) #roll()是循环移位函数。此调用表示向右循环移动2位。

##  **2.数组的特征信息**

先假设已经存在一个N维数组X了，那么可以得到X的一些属性，这些属性可以在输入X和一个.之后，按tab键查看提示。这里明显看到了 [ python
](http://lib.csdn.net/base/python "Python知识库") 面向对象的特征。

  * X.flags #数组的存储情况信息。 
  * X.shape #结果是一个tuple，返回本数组的行数、列数、…… 
  * X.ndim #数组的维数，结果是一个数 
  * X.size #数组中元素的数量 
  * X.itemsize #数组中的数据项的所占内存空间大小 
  * X.dtype #数据类型 
  * X.T #如果X是矩阵，发挥的是X的转置矩阵 
  * X.trace() #计算X的迹 
  * np.linalg.det(a) #返回的是矩阵a的行列式 
  * np.linalg.norm(a,ord=None) #计算矩阵a的范数 
  * np.linalg.eig(a) #矩阵a的特征值和特征向量 
  * np.linalg.cond(a,p=None) #矩阵a的条件数 
  * np.linalg.inv(a) #矩阵a的逆矩阵 

##  **3.矩阵分解**

常见的矩阵分解函数，numpy.linalg均已经提供。比如cholesky()/qr()/svd()/lu()/schur()等。某些 [ 算法
](http://lib.csdn.net/base/datastructure "算法与数据结构知识库")
为了方便计算或者针对不同的特殊情况，还给出了多种调用形式，以便得到最佳结果。

##  **4.矩阵运算**

np.dot(a,b)用来计算数组的点积；vdot(a,b)专门计算矢量的点积，和dot()的区别在于对complex数据类型的处理不一样；innner(a,b)用来计算内积；outer(a,b)计算外积。

专门处理矩阵的数学函数在numpy的子包linalg中定义。比如np.linalg.logm(A)计算矩阵A的对数。可见，这个处理和MATLAB是类似的，使用一个m后缀表示是矩阵的运算。在这个空间内可以使用的有cosm()/sinm()/signm()/sqrtm()等。其中常规exp()对应有三种矩阵形式：expm()使用Pade近似算法、expm2()使用特征值分析算法、expm3()使用泰勒级数算法。在numpy中，也有一个计算矩阵的函数：funm(A,func)。

##  **5.索引**

  * numpy中的数组索引形式和Python是一致的。如： 
  * x=np.arange(10) 
  * print x[2] #单个元素，从前往后正向索引。注意下标是从0开始的。 
  * print x[-2] #从后往前索引。最后一个元素的下标是-1 
  * print x[2:5] #多个元素，左闭右开，默认步长值是1 
  * print x[:-7] #多个元素，从后向前，制定了结束的位置，使用默认步长值 
  * print x[1:7:2] #指定步长值 
  * x.shape=(2,5) #x的 **shape** 属性被重新赋值，要求就是元素个数不变。2*5=10 
  * print x[1,3] #二维数组索引单个元素，第2行第4列的那个元素 
  * print x[0] #第一行所有的元素 
  * y=np.arange(35).reshape(5,7) # **reshape()** 函数用于改变数组的维度 
  * print y[1:5:2,::2] #选择二维数组中的某些符合条件的元素 

#  2、数组和矩阵的区别：

参考 [ http://blog.csdn.net/vincentlipan/article/details/20717163
](http://blog.csdn.net/vincentlipan/article/details/20717163)

Numpy matrices必须是2维的,但是 numpy arrays (ndarrays) 可以是多维的（1D，2D，3D····ND）.
Matrix是Array的一个小的分支，包含于Array。所以matrix 拥有array的所有特性。

在numpy中matrix的主要优势是： **相对简单的乘法运算符号。** 例如，a和b是两个matrices，那么a*b，就是矩阵积。


​    
    import numpy as np
    a=np.mat('4 3; 2 1')  #
    b=np.mat('1 2; 3 4')  #
    print(a)
    # [[4 3]
    # [2 1]]
    
    print(a*b)   #正常的矩阵积
    # [[13 20]
    # [ 5 8]]

matrix 和 array 都可以通过objects后面加.T 得到其转置。但是 matrix objects 还可以在后面加 .H f得到共轭矩阵, 加
.I 得到逆矩阵。

相反的是在numpy里面arrays遵从逐个元素的运算，所以array：c 和d的c*d运算相当于matlab里面的c.*d运算。


​    
    c=np.array([[4, 3], [2, 1]])
    d=np.array([[1, 2], [3, 4]])
    print(c*d)  #对array来说，*意味着对应元素相乘
    # [[4 6]
    # [6 4]]
    
    #而array应用矩阵乘法，则需要numpy里面的dot命令 :
    print(np.dot(c,d))
    # [[13 20]
    # [ 5 8]]

**  运算符的作用也不一样 ：


​    
    print(a**2)
    # [[22 15]
    # [10 7]]
    print(c**2)
    # [[16 9]
    # [ 4 1]]


​    
​      
    #因为a是个matrix，所以a**2返回的是a*a，相当于矩阵相乘。而c是array，c**2相当于，c中的元素逐个求平方

问题就出来了，如果一个程序里面既有matrix 又有array，会让人脑袋大。 **
但是如果只用array，你不仅可以实现matrix所有的功能，还减少了编程和阅读的麻烦。  **

**两者之间的转换：**

> ** np.asmatrix  ** 和  np.asarray

对我来说，numpy 中的array与numpy中的matrix，matlab中的matrix的最大的不同是，在
**做归约运算时，array的维数会发生变化，但matrix总是保持为2维** 。例如下面求平均值的运算

>>> m  =  np.mat([[  1  ,  2  ],[  2  ,  3  ]])

>>> m

matrix([[  1  ,  2  ],

[  2  ,  3  ]])

>>> mm  =  m.mean(  1  )

>>> mm

matrix([[  1.5  ],[  2.5  ]])

>>> mm.shape

(  2  ,  1  )

>>> m  \-  mm

matrix([[  \-  0.5  ,  0.5  ], [  \-  0.5  ,  0.5  ]])

对array 来说

>>> a  =  np.array([[  1  ,  2  ],[  2  ,  3  ]])

>>> a

array([[  1  ,  2  ], [  2  ,  3  ]])

>>> am  =  a.mean(  1  )

>>> am.shape (  2  ,)

>>> am array([  1.5  ,  2.5  ])

>>> a  \-  am

#wrong array([[  \-  0.5  ,  \-  0.5  ],

[  0.5  ,  0.5  ]])

>>> a  \-  am[:, np.newaxis] #right

array([[  \-  0.5  ,  0.5  ],[  \-  0.5  ,  0.5  ]]

#  3、参考网站

numpy相关学习指南： [ https://github.com/rougier/numpy-tutorial
](https://github.com/rougier/numpy-tutorial)

数组矩阵区别 [ http://blog.csdn.net/vincentlipan/article/details/20717163
](http://blog.csdn.net/vincentlipan/article/details/20717163)

科学计算：Python VS. MATLAB(3)----线性代数基础 [
http://blog.sina.com.cn/s/blog_5f234d4701012p64.html
](http://blog.sina.com.cn/s/blog_5f234d4701012p64.html
"http://blog.sina.com.cn/s/blog_5f234d4701012p64.html")

[ python的常见矩阵运算
](http://blog.csdn.net/taxueguilai1992/article/details/46581861)


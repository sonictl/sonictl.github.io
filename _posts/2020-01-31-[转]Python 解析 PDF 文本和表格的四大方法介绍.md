---
layout: post
title: '[转]Python 解析 PDF 文本和表格的四大方法介绍'
date: 2020-01-31 15:08:00 +0800
category: from_cnblogs
slug: p20200131150800
---
# Python 解析 PDF 文本和表格的四大方法介绍

==  <span style='color:red'> code </span> for paper and NSFC Proj. parsing==: https://gitee.com/sonica/pdf_parsing

## 看到一个不错的知识文章，和大家分享一下：

很多文件为了安全都会存成 PDF 格式，比如有的论文、技术文档、书籍等等，程序读取这些文档内容带来了很多麻烦。Python 目前解析 PDF 的扩展包有很多，这里将对比介绍 PyPDF2、pdfplumber、pdfminer3k 以及 Camelot，告诉你哪个是好用的 PDF 解析工具。
本文使用的案例 PDF 文档下载链接：
链接：

[https://pan.baidu.com/s/1zH7vY47AqBYKM0XbdABbUA](https://support.i-search.com.cn/forward?goto=https%3A%2F%2Fpan.baidu.com%2Fs%2F1zH7vY47AqBYKM0XbdABbUA)
提取码：xhem

另外，获取 PDF 文档之后，会发现 PDF 文档中的换行符是以行的位置相同的，而不是跟段落相同。

## 1. PyPDF2 解析 PDF 文档

这里主要参考了 2019-03-07，Usman Malik 写的一篇文章：

Python for NLP: Working with Text and PDF Files

使用 Python 安装 PyPDF2 扩展包：

```
pip install PyPDF2
#---------OR
conda install -c conda-forge pypdf2
```

读取 PDF 文件

```
import PyPDF2
path = r"****.pdf"
#使用open的‘rb’方法打开pdf文件（这里必须得使用二进制rb的读取方式）
mypdf = open(path,mode='rb')
#调用PdfFileReader函数
pdf_document = PyPDF2.PdfFileReader(mypdf)
#使用pdf_document变量，获取各个信息
#或者PDF文档的页数
pdf_document.numPages
#输出PDF文档的第一页内容
first_page = pdf_document.getPage(0)
print(first_page.extractText())
```

输出文档第一页内容之后会发现，PyPDF2 方法对中文的支持不好，而对英文的支持会很好，所以如果处理中文文档的话，可以使用下面这个方法。

## 2. pdfplumber 解析 PDF 文档

安装的话直接使用下面语句即可：

```
pip install pdfplumber
```

（1）解析文本内容
pdfplumber 中的 extract_text 函数是可以直接识别 PDF 中的文本内容。
首先读取整个 PDF 文档文本内容

```
import pdfplumber
import pandas as pd
with pdfplumber.open(path) as pdf: 
    content = ''
    #len(pdf.pages)为PDF文档页数
    for i in range(len(pdf.pages)):
      #pdf.pages[i] 是读取PDF文档第i+1页
        page = pdf.pages[i]
        #page.extract_text()函数即读取文本内容，下面这步是去掉文档最下面的页码
        page_content = '\n'.join(page.extract_text().split('\n')[:-1])
        content = content + page_content
    print(content)
```

解析文本内容，取出 PDF 的售后解决方案中的故障代码内容，可以看到故障代码内容，如下图所示，故障代码在两页里面。

![Python 解析 PDF 文本和表格的四大方法介绍](http://support.i-search.com.cn/upload/bbs/20190418/ce3fde03702544f58100d408b5e044da_1.jpg)
根据这类文档的规律可以知道，故障代码内容都是在文本故障代码列举如下：和 2. 之间，因此解析 PDF 之后取出这部分内容还是比较容易的：

```
print(content.split('故障代码列举如下：')[1].split('2.')[0])
```

运行结果如下，可以看出来很好的取出来这部分内容了。

![Python 解析 PDF 文本和表格的四大方法介绍](http://support.i-search.com.cn/upload/bbs/20190418/4ce9ae24ce154a89bae33703022db8fb_3.jpg)

（2）解析表格内容
上面介绍了 pdfplumber 解析文本内容的方法，这里介绍一下解析表格内容的方法，和上面十分类似，pdfplumber 中的 extract_tables 函数是可以直接识别 PDF 中的表格的。
这里展示解析 PDF 文档中第一页表格的方法，可以看出案例 PDF 中第一页的开头就是一个表格：![Python 解析 PDF 文本和表格的四大方法介绍](http://support.i-search.com.cn/upload/bbs/20190418/47d055e54b6446eb8c3cef58716aa1fb_4.jpg)

由于使用 extract_tables 函数得到的是 Table 一个嵌套的 List 类型，转化成 DataFrame 会更方便查看和分析。

```
import pdfplumber
import pandas as pd

with pdfplumber.open(path) as pdf:      
    first_page = pdf.pages[0]
    for table in first_page.extract_tables(): 
        df = pd.DataFrame(table)
df
```

可以看出这个函数非常容易的将 PDF 文档中的表格提取出来了。

![Python 解析 PDF 文本和表格的四大方法介绍](http://support.i-search.com.cn/upload/bbs/20190418/1c82ef731e0a4ec4a15615cbb6355279_5.jpg)
看完上面的可以知道 pdfplumber 扩展包可以非常好的解析 PDF 的文本内容和表格内容，并且对中文有很好的支持，十分推荐使用该方法。

## 3. pdfminer3k 解析 PDF 文档

pdfminer3k 是 pdfminer 的 python3 版本，主要用于读取 pdf 中的文本。如果直接搜索 pdfminer3k 的话会发现网上有非常多的教程，但是看了之后，你可能就想吐槽这些教程太繁琐了，看着头疼。

下面这个是 pdfminer 解析 PDF 文档的流向图。

![Python 解析 PDF 文本和表格的四大方法介绍](http://support.i-search.com.cn/upload/bbs/20190418/0fb3baed3873495b883b33a33718aad3_6.jpg)

pdfminer 方法解析 PDF 可以很好的提取文本内容，但是对于表格数据，能提取出文字，但是没有格式，会很不友好。因此你如果只需要提取文本内容的话，可以使用 pdfminer 扩展包，这个包也能很好的支持中文。**中文需要考虑编码和全角半角问题。**

## 4. Camelot 解析 PDF 文档

安装
Camelot 先使用 pip install camelot-py 语句安装，如果报错，参考安装 Camelot 教程。
另外，使用 camelot 需要安装 cv2 包，上面这个安装教程中也有。

```
import camelot
import pandas as pd
tables = camelot.read_pdf(filepath=path,pages='1',flavor='stream')
df = pd.DataFrame(tables[0].data)
```

Camelot 读取 PDF 文件中的表格数据很好用，并且能够很好的支持中文，但是 Camelot 有很多局限性。

首先，使用 stream 时，表格无法被自动侦测到，stream 把整个页面当成一个 table。
其次，camelot 只用使用基于文本的 PDF 文件而不能使用扫描文档。

综上所述，建议使用 pdfplumber 扩展包来解析 PDF 文档的文本和表格，如果只解析文本内容，也可以使用 pdfminer ，而解析英文文档内容，可以使用 PyPDF2 。

### read more:
 - [使用Python第三方库pdfminer提取PDF内容，并解决中文编码不支持的问题](https://zhuanlan.zhihu.com/p/29410051)
 - [三大神器助力Python提取pdf文档信息](https://cloud.tencent.com/developer/article/1395339)
 - [python 获取PDF文字（PDFminer)](https://blog.csdn.net/weixin_42983055/article/details/97612809)
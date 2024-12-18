---
layout: post
title:  "Mac Office 对 Windows Office 中文字体不兼容的问题"
date: 2024-12-02 22:27:23 +0800
tags:  
slug: p20241202222723
---

# Mac Office 对 Windows Office 中文字体不兼容的问题

下载的 ttf 字体，或者是从windows 中导出的字体，当被装入Mac 系统 的 Font Book 以后，发现在Mac Office 提供的字体列表中，字体的名称不再是你在windows 中看到的 “方正仿宋\_GBK“  或  “方正仿宋\_GB2312“。

这是因为 Mac 操作系统把 ttf 字体的 metaData (元数据) 中的字段值的 en 部分直接提取出来，显示在了 Office 或者 FontBook App 中。而Windows 操作系统用的是metaData (元数据) 中的字段值的 cn 部分的名称。二者不对应，导致字体不兼容。

### 查看 ”元数据“ 的方法：

Mac Terminal 安装 `fontconfig`

```
brew install fontconfig
```

使用fc-query命令查看字体信息：

``````
fc-query /path/to/your/font.ttf
``````

可见，Windows Word 中的字体设置，采用的是字体的 cn 名称，Mac Fontbook/Word 中，采用的是字体的 en 名称。



### 修改 ”元数据“ 的方法：

或借助在线网站： https://products.aspose.app/font/zh/metadata/ttf  

可以修改字体的元数据。

Mac Office 会将 word 文档中的字体名，与字体库中的字体名进行对应。找到最接近的字体名给它赋予字体进行显示。

以 "方正仿宋\_GBK" 字体为例：

|          | Mac中的字体名   | 元数据中的字体名 | Mac Office 是否能对应上？ |
| -------- | --------------- | ---------------- | ------------------------- |
| 修改前： | FZFangSong_Z02  | FZFangSong_Z02   | ❌                         |
| 修改后： | FZFangSong\_GBK | FZFangSong\_GBK  | ✅                         |

### 解决方法

1、将 windows 版 某字体的 ttf 文件上传至 aspose.app 网站，查看它的元数据。

2、修改元数据，使之字体名尽量符合 windows 字体的全拼，或直接用中文（我没有试）。

3、下载修改元数据后的ttf字体文件，导入Mac系统的FontBook App（网速慢可以上了网再下载）。

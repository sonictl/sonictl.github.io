---
layout: post
title:  "解决 Mac Office 对 Windows Office 中文字体不兼容的问题"
date: 2024-12-02 22:27:23 +0800
tags:  
slug: p20241202222723
---

# 解决 Mac Office 对 Windows Office 中文字体不兼容的问题

你下载的 ttf 字体，或者是从windows 中导出的字体，当你装入Mac 系统 的 Font Book 以后，发现在Mac Office 提供的字体列表中，字体的名称不再是你在windows 中看到的 “方正仿宋\_GBK“  或  “方正仿宋\_GB2312“。

这是因为 Mac 操作系统把 ttf 字体的 metaData (元数据) 中的字段值直接提取出来，给你显示在了 Office 或者 FontBook App 中。

### 你既然知道了 ”元数据“ 的问题，那么应该知道解决思路了

我们有一个在线网站： https://products.aspose.app/font/zh/metadata/ttf  

它可以解决元数据的修改的问题。

Mac Office 会将 word 文档中的字体名，与字体库中的字体名进行对应。找到最接近的字体名给它赋予字体进行显示。

以 "方正仿宋\_GBK" 字体为例：

|          | Mac中的字体名   | 元数据中的字体名 | Mac Office 是否能对应上？ |
| -------- | --------------- | ---------------- | ------------------------- |
| 修改前： | FZFangSong_Z02  | FZFangSong_Z02   | ❌                         |
| 修改后： | FZFangSong\_GBK | FZFangSong\_GBK  | ✅                         |

### 解决方法

1、将 windows 版 某字体的 ttf 文件上传的 aspose.app 网站，查看它的元数据。

2、修改元数据，使之字体名尽量符合 windows 字体的全拼，或直接用中文（我没有试）。

3、下载修改后的元数据，导入Mac系统的FontBook App（网速慢可以上了网再下载）。

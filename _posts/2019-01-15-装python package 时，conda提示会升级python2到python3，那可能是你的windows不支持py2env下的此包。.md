---
layout: post
title: '装python package 时，conda提示会升级python2到python3，那可能是你的windows不支持py2env下的此包。'
date: 2019-01-15 13:09:00 +0800
category: from_cnblogs
slug: p20190115130900
---
# 装python package 时，conda提示会升级python2到python3，

那可能是你的windows不支持py2env下的此包。比如：win 下，tensorflow就不支持py2的环境。你要装tf就必须升级python2 到 python3. 而代码又是用python2 写的，就出这问题了。
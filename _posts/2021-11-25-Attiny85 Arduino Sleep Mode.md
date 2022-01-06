---
layout: post
title:  "Attiny85 Arduino SleepMode 睡眠模式使用笔记"
date: 2021-11-26 18:06:42 +0800
categories: jekyll
slug: p20211126182011
---
## Attiny85 Arduino SleepMode 睡眠模式使用笔记

Attiny 85 board:

![image-20211126180807011](/Users/sonic/Blog_sonictl.github.io/Posts_for_publish/assets/images/image-20211126180807011.png)

这款迷你开发板带了6个IO口，4个ADC，I2C通信结构等资源，已经能应付很多控制场景了。

使用VScode + platformIO ， 在Arduino框架下进行开发。睡眠模式探究







------



### step1: 新增或编辑博文：

<span style="color:red">**cmmad + shift +S** 复制本模板</span>，**再**编辑内容。。

**添加图片：**

添加图片到 /assets/images  会自动同步到  jekyll_proj_root/assets/images, 博文的图片地址：`/assets/images/***.jpg` 。本地预览会不显示，要预览可在图片path前加个“.” 

参考md code: ` ![img](/asserts/images/image-20200523877.png)` 

`![Tux, the Linux mascot](/assets/images/tux.png)`



### step2: 保存blog：

- 若： **非私密博文**：保存到 `public_posts`， 运行 `sync folders` app，自动同步到`jekylly_proj/_posts`
-  若： **私密博文**：保存到 `local_private_posts` 



### stpe3: 生成jekyll web站点以更新

```
cd /Users/sonic/Blog_sonictl.github.io/jekylly_proj
bundle exec jekyll serve # --watch
```

or

```bash
cd /Users/sonic/Blog_sonictl.github.io
./2. run_jekyll_local_svr.command  # this bash script will build web and start jekyll service: localhost:4000
```



## 非私密博文发布到web:

运行2个.command文件即可:

```bash
cd /Users/sonic/Blog_sonictl.github.io
./3. git_pull_merge_local.command    # merge the remote comments data with local firstly
./4. publish_to_github_page.command  # git add . && git commit && git push
```

or in Terminal：

```bash
cd /Users/sonic/Blog_sonictl.github.io/jekylly_proj
bundle exec jekyll serve # --watch
#new Terminal Tab (⌘T):
git status   # for check
git add . && git commit -m 'new cmit' && git push -u origin main
```


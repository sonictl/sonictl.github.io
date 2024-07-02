---
layout: post
title:  "My first Hugo post"
date: 2023-08-01 20:37:12 +0800
categories: DailyLife
slug: p20230801203712
---



### Below is installing and creating a hugo site with theme=fluency

with operations like:

```
    hugo new site sonictl.github.io
    cd sonictl.github.io
    git submodule add https://github.com/wayjam/hugo-theme-fluency.git themes/fluency
    echo "theme = 'fluency'" >> hugo.toml
    hugo server
    
```
by following this [https://gohugo.io/getting-started/quick-start/](https://gohugo.io/getting-started/quick-start/)

##  add blog content:

**directly add a .md file in `content/posts` **

**or ask hugo to creat one post:**

```
hugo new posts/my-first-post.md

```

this create a new file under `content/posts`

Note the draft value in the front matter of your post.

if its value == true, You can run either of the following commands to include draft content.

```
hugo server --buildDrafts
hugo server -D
```

The cmd `hugo server ` will start the blog web server locally and let your checkout.




**2023-08-01 17:52:11**

## Theme and configuration

I am using `fluency` theme for now, but you may need to transfer the `yaml` spec configurations into `toml` spec, you can search online with the `yaml to toml`.

About the theme:

```
The theme folder is actually a git submodule that I cloned directly from the hugo-classic theme GitHub repository, I don’t want to edit any files under that folder directly. If I were to edit files there, I would have uncommitted changes to my git submodule, and that can become a pain to manage being the submodule is not based off of a repository I own or want to commit changes to.theme folder is actually a git submodule that I cloned directly from the hugo-classic theme GitHub repository, I don’t want to edit any files under that folder directly. If I were to edit files there, I would have uncommitted changes to my git submodule, and that can become a pain to manage being the submodule is not based off of a repository I own or want to commit changes to.
```

Refer: [setting-up-hugo-to-use-google-analytics](http://cloudywithachanceofdevops.com/posts/2018/05/17/setting-up-google-analytics-on-hugo/#setting-up-hugo-to-use-google-analytics)



## Publish into the Internet with `git` commands



Ref [https://gohugo.io/hosting-and-deployment/hosting-on-github/](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

Note you may use the "workflow" mechanism of github to deploy your site.

I met the error about `.gitsubmodule` saying that my `hugo_eiio` theme is not added into it. fix it and note to add theme with `git add submodule` rather than `git clone` in the future. See: [submodule for hugo themes](https://www.andrewhoog.com/post/git-submodule-for-hugo-themes/)

Refer about deploy Hugo site with `workflow` onto github pages: https://gohugo.io/hosting-and-deployment/hosting-on-github/



## More about modifiying static artial to enable Google Analytics

The way Hugo handles `partials` is that it looks in two places in a specific order to figure out what needs to be used. That order is:

```bash
1. layouts/partials/*<PARTIALNAME>.html
2. themes/<THEME>/layouts/partials/*<PARTIALNAME>.html
```

So,  we can create a copy of the theme’s `header.html` or `head.html` file, throw it in our `layouts/partials/` folder in the root of our Hugo site, and it will **override** the `header.html` file in our theme’s `layouts/partials` folder **without overwriting it**. Powerful stuff!

Once we have that new `header.html` file, we can add the code to it that will enable the internal Hugo template for Google Analytics on our site without disturbing the code of the theme. 

Refer: [setting-up-hugo-to-use-google-analytics](http://cloudywithachanceofdevops.com/posts/2018/05/17/setting-up-google-analytics-on-hugo/#setting-up-hugo-to-use-google-analytics)



## Add pictures in blog post (添加图片)

图片的本地存放路径：`hugo_root_path/static/img/`。

上传以后，图片的远程路径为：`root_url_of_blog/img/`

此时有两种写法可以在博客中refer图片：

1. **远程url引用方式**，引用路径即绝对路径：`![test_image](https://sonictl.github.io/img/xxx.png)`

2. 推荐：**本地引用方式**，引用路径即相对路径：`![test_image](/img/xxx.png)`:

![test_image](https://sonictl.github.io/img/android-chrome-192x192.png)

```
上图即为远程引用方式 (refer by remote URL)，这种方式的好处是支持本地预览: 
   ![test_image](https://sonictl.github.io/img/android-chrome-192x192.png)
```
**相对路径引用方式，好处是支持博客域名的日后更换:**

![test_image](/img/android-chrome-192x192.png)

```markdown
![test_image](/img/android-chrome-192x192.png)
```

You'll see this picture online, but locally invisible.

**English:**

 in your `Hugo_root/static/img/img_test.png` , the picture will be online at `your_domain_name_of_blog/img/img_test.png` . So, you can manage your image's path:

![test_image](https://sonictl.github.io/img/android-chrome-192x192.png)



Here I use:

 `https://sonictl.github.io/img/android-chrome-192x192.png`

 for a picture saved at:

` <hugo Root Folder>/static/img/android-chrome-192x192.png`


![test_image](/img/android-chrome-192x192.png)

This is also work (**Recommended**): 

```markdown
![test_image](/img/android-chrome-192x192.png)
```



## References:

References: for deeper Hugo usage:

https://gohugo.io/content-management/page-resources/
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



## Publish into the Internet with `git` commands



Ref [https://gohugo.io/hosting-and-deployment/hosting-on-github/](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

Note you may use the "workflow" mechanism of github to deploy your site.

I met the error about `.gitsubmodule` saying that my `hugo_eiio` theme is not added into it. fix it and note to add theme with `git add submodule` rather than `git clone` in the future. See: [submodule for hugo themes](https://www.andrewhoog.com/post/git-submodule-for-hugo-themes/)

Refer about deploy Hugo site with `workflow` onto github pages: https://gohugo.io/hosting-and-deployment/hosting-on-github/



## Todo:

 [ ] change icon of tag.

 [ ] add comment.

 [ ] beautilize, like pictures.




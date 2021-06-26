---
layout: page
title: About
permalink: /about/
---

This is the base Jekyll theme. You can find out more info about customizing your Jekyll theme, as well as basic Jekyll usage documentation at [jekyllrb.com](https://jekyllrb.com/)

You can find the source code for Minima at GitHub:
[jekyll][jekyll-organization] /
[minima](https://github.com/jekyll/minima)

You can find the source code for Jekyll at GitHub:
[jekyll][jekyll-organization] /
[jekyll](https://github.com/jekyll/jekyll)


[jekyll-organization]: https://github.com/jekyll

```
basic steps for adding post for REFERENCE:

in Terminal, cd to ~/sonictl.github.io
in Terminal, run 'bundle exec jekyll serve --watch'
do you modifying/adding the markdowns, save. check in localhost:4000
in Terminal, git status/add ./commit -m 'cmt'/push -u origin main
done.

```

### basic format for new post

```
---
layout: post
title:  "Title"
date:   2021-06-26 07:57:34 +0800
categories: jekyll update
---

fileName: 2021-06-26-welcome-to-jekyll.md

```





### **Quick setup** — if you’ve create a new repository

[ Set up in Desktop](x-github-client://openRepo/https://github.com/sonictl/sonictl.github.io)

Get started by [creating a new file](https://github.com/sonictl/sonictl.github.io/new/main) or [uploading an existing file](https://github.com/sonictl/sonictl.github.io/upload). We recommend every repository include a [README](https://github.com/sonictl/sonictl.github.io/new/main?readme=1), [LICENSE](https://github.com/sonictl/sonictl.github.io/new/main?filename=LICENSE.md), and [.gitignore](https://github.com/sonictl/sonictl.github.io/new/main?filename=.gitignore).

### …or create a new repository on the command line



```
echo "# sonictl.github.io" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/sonictl/sonictl.github.io.git
git push -u origin main
```

### …or push an existing repository from the command line



```
git remote add origin https://github.com/sonictl/sonictl.github.io.git
git branch -M main
git push -u origin main
```

### …or import code from another repository

You can initialize this repository with code from a Subversion, Mercurial, or TFS project.

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

check the [source code](https://github.com/sonictl/sonictl.github.io) for [this blog](https://sonictl.github.io).

```
basic steps for adding post for REFERENCE:

in Terminal, cd <to the jekyll proj dir>
in Terminal, run 'bundle exec jekyll serve --watch'  # or: bundle exec jekyll serve
do you modifying/adding the markdowns, save. check in localhost:4000
in Terminal, git status/add ./commit -m 'cmt'/push -u origin main
done.

```
**Quick reference:**
```
cd <to the jekyll proj dir> && bundle exec jekyll serve
cd <to the jekyll proj dir> && git add . && git commit -m 'new cmit' && git push -u origin main
```

### basic format for new post

```
---
layout: post
title:  "Title"
date:   2021-06-26 07:57:34 +0800
categories: jekyll tech
---

fileName: 2021-06-26-welcome-to-jekyll.md

```

## solve timeZone issue for jekyll:
```
- write "date: 2019-03-17 12:07:00 +0800" in frontMatter of each post 
```

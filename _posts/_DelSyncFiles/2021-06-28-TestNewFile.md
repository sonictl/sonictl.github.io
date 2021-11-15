---
title: '如何更新博文'
date: 2021-07-04 22:01:00
---

### step1: 新增或编辑博文：

**cmmad + shift +S** 复制本md文件，编辑，然后：

- **在线发布到github page**： `public_posts` 运行 `sync folders` app同步到`jekylly_public/_posts`

  或者

-  **仅保存到本地**： `local_private_posts` 

### step2: 发布到web:

Terminal运行：

```bash
cd /Users/sonic/Blog_sonictl.github.io/jekylly_public
bundle exec jekyll serve
#new Terminal window:
git status   # for check
git add . && git commit -m 'new cmit' && git push -u origin main
```


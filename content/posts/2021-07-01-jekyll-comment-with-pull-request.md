---
layout: post
title:  "Implementing comment for jekyll blog with github pull request"
date: 2021-07-01 17:01:00 +0800
categories: jekyll
slug: p20210701170100
---

# 利用pull request实现 jekyll 博客评论功能
## Implementing comment for jekyll blog with github pull request


#### 1. The refered blogs: 

- https://damieng.com/blog/2018/05/28/wordpress-to-jekyll-comments
- https://haacked.com/archive/2018/06/24/comments-for-jekyll-blogs/

The github repos that needed:

- https://github.com/damieng/jekyll-blog-comments This is for the static layout/html contents supporting  comment functions.
- https://github.com/haacked/jekyll-blog-comments-azure This is for the dynamic Azure Function code, which is forked from: https://github.com/Azure-Functions/jekyll-blog-comments

---

#### 2. Steps and notes for jekyll comment box building

   The idea is use [data files](https://jekyllrb.com/docs/datafiles/) in Jekyll to store comments and some liquid templates to render them. That all can be static.

   Azure Function or AWS Lambda combined with the GitHub API. Receive a comment form submission and create the appropriate data file in your Jekyll repository.

   Damien Guard wrote an [Azure function](https://damieng.com/blog/2018/05/28/wordpress-to-jekyll-comments) that calls the GitHub API using [Octokit.net](https://github.com/octokit/octokit.net) (`Install-Package octokit` when using NuGet) to create a Pull Request that contains a data file with the content info rendered as yaml.

   Following the instructions in: https://github.com/damieng/jekyll-blog-comments to set up.

   ref: Damien's blog about this topic: https://damieng.com/blog/2018/05/28/wordpress-to-jekyll-comments

   ```text
   1. create test data and attempt rendering: 
      create three htmls in /_includes: comment.html, new-comment.html, comments.html
    - Render an individual comment (comment.html)
    - Show a form to accept a new comment (new-comment.html)
    - Loop over individual comments for a post (comments.html)
      
   2. modify the blog post template file: _layouts/post.html
    - add 'include comments.html' after line of `page.content`
    @@: should add 'include item.html' before line of `page.content`, but the item.html is not offered by jekyll-blog-comments repo. And jekyll report file not exist error. `jekyll clean` to clear the built proj.
    
   3. add the following line to _config.yml:
      - add `emptyArray: [] `
    
   4. add /comments folder under jekylls /_data folder
      
   ```

   store each comment in a file named `_data/{blog_post_slug}/{comment_id}.yml` with this format:

   blog_post_slug 可以在frontMatter里指定：`slug: yourSlug` 或者参考_includes/comments.html 第一行：

`{%raw%}{% capture default_slug %}{{ page.slug | default: (page.title | slugify) }}{% endcapture %}{%endraw%}`

其中，page.title 被认为是默认的slug. 

   ```
   id: 12345
   name: Damien Guard
   email: damieng@gmail.com
   gravatar: dc72963e7279d34c85ed4c0b731ce5a9
   url: https://damieng.com
   date: 2007-12-18 18:51:55
   message: "This is a great solution for 'dynamic' comments on a static blog!"
   ```

   **Accepting new comments with an Azure function**

   需要安装[jekyll-blog-comments-azure](https://github.com/damieng/jekyll-blog-comments-azure)， 先fork, 再：

   ![image-20210630225938194](/assets/images/image-20210630225938194.png)

   Create Azure Account, and create aZure function

   fork the github repo: https://github.com/damieng/jekyll-blog-comments

   1. [Create a **v1** Azure Function](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-first-azure-function)

      change the runtime through the Configuration screen of the Function App by changing the FUNCTIONS_EXTENSION_VERSION key to ~1.

      ref: https://marcroussy.com/2018/11/10/create-runtime-v1-azure-functions/

   2. [Set up your function to deploy from your fork](https://docs.microsoft.com/en-us/azure/azure-functions/scripts/functions-cli-create-function-app-github-continuous)

      deploy jekyll-blog-comments:

      deploy with the CLI code, configure before running as a executable .sh file.

   3. Set up the following [App Settings for your Azure Function](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings)

      You also need to set-up two **Application Settings** for your function so it can create the necessary pull requests. They are:

      - `GitHubToken` should be a [personal access token](https://github.com/settings/tokens) with `repo` rights

      - `PullRequestRepository` should contain the org and repo name, e.g. `damieng/my-blog` 

        **Note:** it should just be in the format `org/repo`, e.g. in your case `urname/urname.github.io`

4. I meet the `handle forms for '$'.` error from `*.azurewebsites.net/api/PostComment`

   ```xml
   <Error>
   <Message>This Jekyll comments receiever does not handle forms for '$'. You should point to your own instance.</Message>
   </Error>
   ```

   in `_config.yml` I missed the url: "\<host url of your blog\>" 

   another issue I met is solved in https://github.com/Azure-Functions/jekyll-blog-comments/issues/16

   

Enjoy~

---

**Note:** After you successfully deploied this comment function, you will need to pull&merge the remote git repo before push local modifications of posts, since the `/_data` folder is changed by pull request created by the jekyll-blog-comments Azure Function.

### Tips:

1. Check and debug by using the background AzureFunction LogStream.

2. for preventing jekyll compile raw code of [Liquid](https://jekyllrb.com/docs/liquid/) templating language.
   

   ![image-20210713172424532](/assets/images/image-20210713172424532.png)

3. if you want to prevent the thanks.md displaying its link in index, under default theme, you can modify the `/includes/header.html`. Add the `{% unless my_page.exclude %}` and `{% endunless %}` surrounding the `<a>` tag that generates the links of thank page. Then, add `exclude: true` in the frontmatter of thanks.md markdown page/code. Reference: [this link](https://stackoverflow.com/questions/25452429/excluding-page-from-jekyll-navigation-bar)


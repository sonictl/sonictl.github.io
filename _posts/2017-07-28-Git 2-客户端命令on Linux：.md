---
layout: post
title: 'Git 2-客户端命令on Linux：'
date: 2017-07-28 14:11:00 +0800
category: from_cnblogs
slug: p20170728141100
---
<h1>Git客户端命令on Linux：</h1>
<p>在此之前：<a id="link_post_title" class="link-post-title" href="http://www.cnblogs.com/sonictl/diary/2017/07/23/7226426.html">Git 1-服务端搭建 Setting up a Git Server in CentOS 6.5</a><br />本地客户端操作的第一步，通常是从远程主机克隆一个版本库，这时就要用到git clone命令，但在自己搭建Git Remote Server的用法中，往往是从git remote add（添加远程主机）开始的。<br />本文与git clone相关的内容被放在第2点。</p>
<h2>1. 管理远程主机</h2>
<h3 style="margin-left: 30px;">a.查看远程主机名</h3>
<p style="margin-left: 30px;">为了便于管理，Git要求每个远程主机都必须指定一个主机名，<span style="color: #808080;">即远程主机在本地的一个代号？</span>。git remote命令就用于管理主机名。</p>
<pre>    &gt;&gt;<span style="color: #000000;"> git remote
    &gt;&gt; git remote -<span style="color: #000000;">v
    origin sonictl@x.x.x.x:/home/git_admin/<span style="color: #000000;">project1 (fetch)
    origin sonictl@x.x.x.x:/home/git_admin/<span style="color: #000000;">project1 (push)

    #使用-<span style="color: #000000;">v选项，显示远程主机的网址。
    &gt;&gt;git remote show &lt;主机名&gt; # 查看主机详细信息：git remote show origin</span></span></span></span></span></pre>
<h4>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ==相关命令==</h4>
<ul>
<li>远程主机的改名。git remote rename命令
<pre class="brush:bash;gutter:true;"> &gt;&gt; $ git remote rename &lt;原主机名&gt; &lt;新主机名&gt;
</pre>
</li>
</ul>
<ul>
<li>添加远程主机。&nbsp;git remote add命令
<pre class="brush:bash;gutter:true;"> &gt;&gt; $ git remote add &lt;主机名&gt; &lt;网址&gt;
 &gt;&gt; $ git remote add origin tony@x.x.x.x:/home/git_admin/project1</pre>
</li>
<li>删除远程主机。git remote rm命令
<pre class="brush:bash;gutter:true;"> &gt;&gt; $ git remote rm &lt;主机名&gt;</pre>
</li>
</ul>
<pre class="brush:bash;gutter:true;">===</pre>
<h2>2. 直接从远程主机克隆版本库</h2>
<p style="margin-left: 30px;"><span style="font-family: courier new,courier;">$ git clone &lt;版本库的网址&gt; </span></p>
<p style="margin-left: 30px;">如：</p>
<div class="cnblogs_Highlighter" style="margin-left: 30px;">
<pre class="brush:bash;gutter:true;">$ git clone https://github.com/jquery/jquery.git
$ git clone sonictl@43.245.223.188:/home/git_admin/project1 localDirName #本地目录名为可选参数
</pre>
</div>
<p style="margin-left: 30px;">&nbsp;git clone支持多种协议，除了HTTP(s)以外，还支持SSH、Git、本地文件协议等，下面是一些例子。</p>
<div class="cnblogs_code" style="margin-left: 30px;" onclick="cnblogs_code_show('042db039-30dc-41f5-993d-b75ae8d7c70d')"><img id="code_img_closed_042db039-30dc-41f5-993d-b75ae8d7c70d" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_042db039-30dc-41f5-993d-b75ae8d7c70d" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('042db039-30dc-41f5-993d-b75ae8d7c70d',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_042db039-30dc-41f5-993d-b75ae8d7c70d" class="cnblogs_code_hide" style="margin-left: 30px;">
<pre>$ git clone http[s]:<span style="color: #008000;">//</span><span style="color: #008000;">example.com/path/to/repo.git/</span>
$ git clone <span style="color: #0000ff;">ssh</span>:<span style="color: #008000;">//</span><span style="color: #008000;">example.com/path/to/repo.git/</span>
$ git clone git:<span style="color: #008000;">//</span><span style="color: #008000;">example.com/path/to/repo.git/</span>
$ git clone /opt/git/<span style="color: #000000;">project.git
$ git clone </span><span style="color: #0000ff;">file</span>:<span style="color: #808080;">///</span><span style="color: #008000;">opt/git/project.git</span>
$ git clone <span style="color: #0000ff;">ftp</span>[s]:<span style="color: #008000;">//</span><span style="color: #008000;">example.com/path/to/repo.git/</span>
$ git clone rsync:<span style="color: #008000;">//</span><span style="color: #008000;">example.com/path/to/repo.git/</span>
<span style="color: #000000;">#SSH协议还有另一种写法:
$ git clone [user@]example.com:path</span>/to/repo.git/<span style="color: #000000;">
#通常来说，Git协议下载速度最快，SSH协议用于需要用户认证的场合。各种协议优劣的详细讨论请参考官方文档。</span></pre>
</div>
<span class="cnblogs_code_collapse">展开显示</span></div>
<p>&nbsp;</p>
<h2>3. Fetch - 取。当远程有更新时</h2>
<p>git fetch命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响。</p>
<h3>&nbsp;&nbsp; 全部取回与指定分支名取回：</h3>
<ol>
<li>将某个远程主机的更新，全部取回本地----------&gt; $ git fetch origin</li>
<li>如果只想取回特定分支的更新，可以指定分支名--&gt; $ git fetch origin master</li>
</ol>
<p>所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如origin主机的master，就要用origin/master读取。<span style="color: #800000;">@@@如何读取？？</span></p>
<h3>&nbsp;&nbsp; 关于分支</h3>
<ul>
<li>查看本地分支：$ git branch</li>
<li>查看远程分支: git branch -r</li>
<li>查看所有分支（本地+远程）: git branch -a</li>
<li>创建一个新的分支：<br />取回远程主机的更新以后，可以在它的基础上，使用git checkout命令创建一个新的分支。<br /><span style="font-family: courier new,courier;">$ git checkout -b newBrach origin/master</span><br />上面命令表示，在origin/master的基础上，创建一个新分支。</li>
<li>在本地分支上合并远程分支: <span style="font-family: courier new,courier;">git merge</span>命令或者<span style="font-family: courier new,courier;">git rebase</span>命令<br /><span style="font-family: courier new,courier;">$ git merge origin/master</span><br /># 或者<br /><span style="font-family: courier new,courier;">$ git rebase origin/master</span><br />上面命令表示在当前分支上，合并origin/master</li>






</ul>
<h2>4. git pull - 拉, fetch &amp; merge</h2>
<p style="margin-left: 30px;">git pull - 取回远程主机某个分支的更新，再与本地的指定分支合并。</p>
<pre class="brush:bash;gutter:true;">$ git pull &lt;远程主机名&gt; &lt;远程分支名&gt;:&lt;本地分支名&gt;<br />比如：取回<code>origin</code>主机的<code>next</code>分支，与本地的<code>master</code>分支合并，需要写成下面这样。</pre>
<blockquote>
<pre class=" language-javascript"><code class=" language-javascript">$ git pull origin next<span class="token punctuation">:master
</span></code></pre>
</blockquote>
<p style="margin-left: 30px;">如果远程分支是与当前分支合并，则冒号后面的部分可以省略。取回<code>origin/next</code>分支，再与当前分支合并：</p>
<blockquote>
<pre class=" language-javascript"><code class=" language-javascript">$ git pull origin next  <br /><br />实质上，这等同于先做<code>git fetch</code>，再做<code>git merge</code>。<br />同下：<br />$ git fetch origin
$ git merge origin<span class="token operator">/next</span></code></pre>
</blockquote>
<h3>&nbsp;&nbsp;&nbsp;&nbsp; 关于追踪</h3>
<p style="margin-left: 30px;">在某些场合，Git会自动在本地分支与远程分支之间，建立一种追踪关系（tracking）。比如，在<code>git clone</code>的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的<code>master</code>分支自动"追踪"<code>origin/master</code>分支。</p>
<p style="margin-left: 30px;">Git也允许手动建立追踪关系。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">git branch <span class="token operator">--set<span class="token operator">-upstream master origin<span class="token operator">/next
</span></span></span></code></pre>
</blockquote>
<p style="margin-left: 30px;">上面命令指定<code>master</code>分支追踪<code>origin/next</code>分支。</p>
<p style="margin-left: 30px;">如果当前分支与远程分支存在追踪关系，<code>git pull</code>就可以省略远程分支名。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git pull origin
</code></pre>
</blockquote>
<p style="margin-left: 30px;">上面命令表示，本地的当前分支自动与对应的<code>origin</code>主机"追踪分支"（remote-tracking branch）进行合并。</p>
<p style="margin-left: 30px;">如果当前分支只有一个追踪分支，连远程主机名都可以省略。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git pull
</code></pre>
</blockquote>
<p style="margin-left: 30px;">上面命令表示，当前分支自动与唯一一个追踪分支进行合并。</p>
<p style="margin-left: 30px;">注：如果合并需要采用rebase模式，可以使用<code>--rebase</code>选项。格式：</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git pull <span class="token operator">--rebase <span class="token operator">&lt;远程主机名<span class="token operator">&gt; <span class="token operator">&lt;远程分支名<span class="token operator">&gt;<span class="token punctuation">:<span class="token operator">&lt;本地分支名<span class="token operator">&gt;</span></span></span></span></span></span></span></span></code></pre>
</blockquote>
<h3 style="margin-left: 30px;">在本地删除远程已经删除的分支</h3>
<p style="margin-left: 30px;">如果远程主机删除了某个分支，默认情况下，在拉取远程分支的时候，<code>git pull</code>&nbsp;不会删除对应的本地分支。这是为了防止：其他人操作了远程主机，导致<code>git pull</code>不知不觉删除了本地分支。</p>
<p style="margin-left: 30px;">但是，你可以改变这个行为，加上参数&nbsp;<code>-p</code>&nbsp;就会在本地删除远程已经删除的分支。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git pull <span class="token operator">-p
# 等同于下面的命令
$ git fetch <span class="token operator">--prune origin 
$ git fetch <span class="token operator">-p</span></span></span></code></pre>
</blockquote>
<h2>5. git push - 推，将本地更新推送到远端</h2>
<p style="margin-left: 30px;"><code>git push</code>命令用于将本地分支的更新，推送到远程主机。它的格式与<code>git pull</code>命令相仿。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git push <span class="token operator">&lt;远程主机名<span class="token operator">&gt; <span class="token operator">&lt;本地分支名<span class="token operator">&gt;<span class="token punctuation">:<span class="token operator">&lt;远程分支名<span class="token operator">&gt;
</span></span></span></span></span></span></span></code></pre>
</blockquote>
<p style="margin-left: 30px;">注意，分支推送顺序的写法是&lt;来源地&gt;:&lt;目的地&gt;，所以<code>git pull</code>是&lt;远程分支&gt;:&lt;本地分支&gt;，而<code>git push</code>是&lt;本地分支&gt;:&lt;远程分支&gt;。</p>
<p style="margin-left: 30px;">如果省略远程分支名，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git push origin master
</code></pre>
</blockquote>
<p style="margin-left: 30px;">上面命令表示，将本地的<code>master</code>分支推送到<code>origin</code>主机的<code>master</code>分支。如果后者不存在，则会被新建。</p>
<p style="margin-left: 30px;">如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git push origin <span class="token punctuation">:master
# 等同于
$ git push origin <span class="token operator">--delete master
</span></span></code></pre>
</blockquote>
<p style="margin-left: 30px;">上面命令表示删除<code>origin</code>主机的<code>master</code>分支。</p>
<p style="margin-left: 30px;">如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git push origin
</code></pre>
</blockquote>
<p style="margin-left: 30px;">上面命令表示，将当前分支推送到<code>origin</code>主机的对应分支。</p>
<p style="margin-left: 30px;">如果当前分支只有一个追踪分支，那么主机名都可以省略。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git push
</code></pre>
</blockquote>
<p style="margin-left: 30px;">如果当前分支与多个主机存在追踪关系，则可以使用<code>-u</code>选项指定一个默认主机，这样后面就可以不加任何参数使用<code>git push</code>。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">
$ git push <span class="token operator">-u origin master
</span></code></pre>
</blockquote>
<p style="margin-left: 30px;">上面命令将本地的<code>master</code>分支推送到<code>origin</code>主机，同时指定<code>origin</code>为默认主机，后面就可以不加任何参数使用<code>git push</code>了。</p>
<p style="margin-left: 30px;">不带任何参数的<code>git push</code>，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用<code>git config</code>命令。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">
$ git config <span class="token operator">--global push<span class="token punctuation">.default matching
# 或者
$ git config <span class="token operator">--global push<span class="token punctuation">.default simple
</span></span></span></span></code></pre>
</blockquote>
<p style="margin-left: 30px;">还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用<code>--all</code>选项。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git push <span class="token operator">--all origin
</span></code></pre>
</blockquote>
<p style="margin-left: 30px;">上面命令表示，将所有本地分支都推送到<code>origin</code>主机。</p>
<p style="margin-left: 30px;">如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做<code>git pull</code>合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用<code>--force</code>选项。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git push <span class="token operator">--force origin 
</span></code></pre>
</blockquote>
<p style="margin-left: 30px;">上面命令使用<code>--force</code>选项，结果导致远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用<code>--force</code>选项。</p>
<p style="margin-left: 30px;">最后，<code>git push</code>不会推送标签（tag），除非使用<code>--tags</code>选项。</p>
<blockquote style="margin-left: 30px;">
<pre class=" language-javascript"><code class=" language-javascript">$ git push origin <span class="token operator">--tags</span></code></pre>
</blockquote>
<p style="margin-left: 30px;">&nbsp;<img title="总结图" src="http://image.beekka.com/blog/2014/bg2014061202.jpg" alt="汇总图" width="800" height="227" border="2" /></p>
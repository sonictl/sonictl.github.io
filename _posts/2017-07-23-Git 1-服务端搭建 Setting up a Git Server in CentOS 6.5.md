---
layout: post
title: 'Git 1-服务端搭建 Setting up a Git Server in CentOS 6.5'
date: 2017-07-23 14:24:00 +0800
category: from_cnblogs
slug: p20170723142400
---
<h1 id="page-title" class="title">Setting up a Git Server in CentOS 6.5</h1>
<h3 class="tabs">Quick jump to: <a title="sonictl" href="#Create_a_git_user">Create a git user account</a></h3>
<div class="region region-content">
<div id="node-157" class="node node-article node-promoted">
<div class="content">
<div class="field field-name-body field-type-text-with-summary field-label-hidden">
<div class="field-item even">
<p>Install git.</p>
<blockquote>
<p>[<a href="mailto:root@svn">root@svn</a>&nbsp;~]# yum install git</p>
</blockquote>
<p>Add the developers group, all git users will be part of this group.</p>
<blockquote>
<p>[<a href="mailto:root@svn">root@svn</a>&nbsp;~]# groupadd developers</p>
</blockquote>
<p>Create the git user which will own all the repos.</p>
<blockquote>
<p>[<a href="mailto:root@svn">root@svn</a>&nbsp;~]# useradd -s /sbin/nologin -g developers git_admin<br />[<a href="mailto:root@svn">root@svn</a>&nbsp;~]# passwd git_admin<br />Changing password for user git.<br />New password: git********<br />Retype new password:git********<br />passwd: all authentication tokens updated successfully.</p>









</blockquote>
<p>Update Permissions.</p>
<blockquote>
<p>[<a href="mailto:root@svn">root@svn</a>&nbsp;~]# chmod 2770 /home/git_admin/</p>









</blockquote>
<p>Create an empty Git repo.</p>
<blockquote>
<p>[<a href="mailto:root@svn">root@svn</a>&nbsp;root]# cd /home/git_admin &amp;&amp; mkdir project1 &amp;&amp; cd project1<br />[<a href="mailto:root@svn">root@svn</a>&nbsp;project1]# git init --bare --shared</p>
<p>Initialized empty shared Git repository in /home/git_admin/project1/</p>









</blockquote>
<p>Update file ownership and permissions.</p>
<blockquote>
<p>[<a href="mailto:root@svn">root@svn</a>&nbsp;project1]# chown -R git_admin .<br />[<a href="mailto:root@svn">root@svn</a>&nbsp;project1]# chmod 2770 /home/git_admin/project1</p>









</blockquote>
<p><strong><a name="Create_a_git_user"></a>======== Create a git user account. =========</strong></p>
<blockquote>
<p>[<a href="mailto:root@svn">root@svn</a>&nbsp;git_admin]# useradd -s /usr/bin/git-shell -g developers -d /home/git_admin tony<br />useradd: warning: the home directory already exists.<br />Not copying any file from skel directory into it.<br />[<a href="mailto:root@svn">root@svn</a>&nbsp;git_admin]# passwd tony<br />Changing password for user tony.<br />New password: git********<br />Retype new password: git********<br />passwd: all authentication tokens updated successfully.</p>









</blockquote>
<p>At this point a regular user should be able to checkout the project1 repo from the Git server.</p>
<blockquote>
<p><span style="color: #0000ff;">tony@apha05:</span>~$ mkdir ~/testing_shit/git_test<br /><span style="color: #0000ff;">tony@apha05:</span>~$ cd ~/testing_shit/git_test &amp;&amp; git init<br />Initialized empty Git repository in /home/ubuntu/testing_shit/git_test/.git/<br /><span style="color: #0000ff;">tony@apha05:~/testing_shit/git_test$</span> git remote add origin&nbsp;tony@xxx.xxx.xxx.xxx:/home/git_admin/project1</p>









</blockquote>
<p>此时，如果你在apha05机器上 ~/testing_shit/git_test 目录下ls -all命令查看文件，会看到.git文件夹。</p>
<p>Now, let's create a readme.txt file in local git directory and commit it into local git repository.</p>
<blockquote>
<p><span style="color: #0000ff;">tony@apha05:</span>~$ cd ~/testing_shit/git_test &amp;&amp; vim readme.txt<br />edit the contet like :" Git's a version control system . --by tony " and save this file.<br /><span style="color: #0000ff;">tony@apha05: git_test</span>$&nbsp;git add readme.txt<br /><span style="color: #0000ff;">tony@apha05: git_test</span>$ git commit -m "wrote a readme file by tony"</p>
<p>[master (root-commit) a058fc9] wrote a readme file by tony<br /> 1 file changed, 1 insertion(+)<br /> create mode 100644 readme.txt</p>









</blockquote>
<p>Next, Push this commit upto remote git server:</p>
<blockquote>
<p><span style="color: #0000ff;">tony@apha05: git_test</span>$ git push origin master<br />The authenticity of host 'xxx.xxx.xxx.xxx' can't be established.<br />RSA key fingerprint is ??:??:??:??:??:??:??:??:??:??:??:??:??:??:??.<br />Are you sure you want to continue connecting (yes/no)? yes<br />Warning: Permanently added 'xxx.xxx.xxx.xxx' (RSA) to the list of known hosts.<br />tony@xxx.xxx.xxx.xxx's password: <br />Could not chdir to home directory /home/git: No such file or directory<br />Counting objects: 3, done.<br />Compressing objects: 100% (2/2), done.<br />Writing objects: 100% (3/3), 299 bytes | 0 bytes/s, done.<br />Total 3 (delta 0), reused 0 (delta 0)<br />To tony@xxx.xxx.xxx.xxx:/home/git_admin/project1<br /> * [new branch]      master -&gt; master</p>









</blockquote>
<p>You can also test via <code>git clone</code> command:</p>
<blockquote><span style="color: #0000ff;">tony@apha05: ~</span>$ git clone tony@xxx.xxx.xxx.xxx:/home/git_admin/project1
</blockquote>
<p>　　</p>
<p><strong>Note:</strong></p>
<p>before push your local repo upto remote server, you can do serveral changes for your code in git repo:</p>
<ul>
<li>git add &lt;file&gt; &nbsp;#add file into git repo</li>
<li>git commit -m "commit notes"</li>
<li>git status #check the status of git repo</li>
<li>git diff #check the differences,&nbsp;如果<code>git status</code>告诉你有文件被修改过，用<code>git diff</code>可以查看修改内容。</li>
<li>git log #display the log</li>
<li>git reset #backward to previous versions</li>
<li>git reflog #check the history of git commands</li>



</ul>
<p>&nbsp;ref:&nbsp;<a title="sonictl" href="http://www.cnblogs.com/GDUT-xiang/p/5706905.html" target="_blank">http://www.cnblogs.com/GDUT-xiang/p/5706905.html</a><br />&nbsp;&nbsp; &nbsp; &nbsp;&nbsp;<a title="sonictl" href="http://www.ruanyifeng.com/blog/2014/06/git_remote.html" target="_blank">Git远程操作详解</a><br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <a href="https://git-scm.com/download/gui/windows" target="_blank">Git Clients</a></p>
<blockquote>#&amp;</blockquote>
<p>&nbsp;</p>
<p>&nbsp;</p>







</div>









</div>









</div>









</div>









</div>
---
layout: post
title: '配置httpd(Apache)服务器'
date: 2017-07-31 10:23:00 +0800
category: from_cnblogs
slug: p20170731102300
---
<p>配置httpd(Apache)使其指向网页首页所在目录</p>
<p>1. apachectl 命令查看apache服务器信息<br />&nbsp; &nbsp; apachectl -t #语法检查</p>
<p>2. 查看httpd系统服务：<br />&nbsp; &nbsp; ll /etc/init.d/httpd<br />&nbsp; &nbsp;如果没有，参考：http://www.cnblogs.com/zzzhfo/p/5925786.html<br />&nbsp; &nbsp; chkconfig --list httpd   #查看httpd服务的自启动状态<br /> <br />2. 将网页文件放到以下地址：<br />&nbsp; &nbsp; /var/www/html"</p>
<p>3. 设置开机启动：chkconfig</p>
<blockquote>
<pre class="prettyprint"><code class="has-numbering">chkconfig --list</code></pre>
<pre class="prettyprint">chkconfig --add httpd</pre>
<pre class="prettyprint">chkconfig httpd on</pre>
</blockquote>
<p>4. 启动服务：<br />&nbsp;&nbsp;&nbsp;&nbsp; /etc/init.d/httpd start<br />&nbsp; &nbsp; &nbsp;/etc/init.d/httpd stop #关闭服务<br />5. 查看服务状态：<br />&nbsp; &nbsp; netstat -anpt | grep httpd</p>
<p>6. 更多配置，见配置文件: /etc/httpd/conf/httpd.conf</p>
<p>&nbsp;</p>
<p>CentOS firewall 对80等端口的block解除：</p>
<blockquote>
<div class="post-text">
<p>on centos run this commands:</p>
<ol>
<li>
<p><span style="font-family: courier new,courier;">iptables -I INPUT 4 -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT</span></p>
</li>
<li>
<p><span style="font-family: courier new,courier;">/etc/init.d/iptables save</span></p>
</li>
</ol></div>
</blockquote>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Link for details: <a href="https://www.upcloud.com/support/configuring-iptables-on-centos-6-5/" target="_blank">https://www.upcloud.com/support/configuring-iptables-on-centos-6-5/</a></p>
<div class="post-text">&nbsp;</div>
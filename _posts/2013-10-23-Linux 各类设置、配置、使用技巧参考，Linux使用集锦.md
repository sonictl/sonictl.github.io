---
title: 'Linux 各类设置、配置、使用技巧参考，Linux使用集锦'
date: 2013-10-23 01:15:00
---
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<h3>【日期】：<br />
《标题》</h3>
<p>参考链接ref1:</p>
<p>参考链接ref2:<br />
</p>
<p>正文：<br />
</p>
<p>&nbsp;</p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<p>&nbsp;</p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<p>【日期：2018年5月25日15:55】</p>
<p><strong><span style="font-size: 14pt;">《<strong>LINUX下，网上下载的已经编译完成的可执行程序如何安装？Ubuntu 18.04</strong>》</span></strong></p>
<p>&nbsp; https://www.youtube.com/watch?v=owQXBsYIC7U</p>
<p>&nbsp; --------</p>
<p>&nbsp; 1. Extract APP to /opt<br />&nbsp; 2. right clicked on Desktop and clicked Create a new launcher here and created Waterfox shortcut<br />&nbsp; 3. copied Waterfox shortcut to /bin and /usr/share/applications<br />&nbsp; 4. logged off and logged back in<br />&nbsp; 5. Google " Create Desktop Shortcut in Linux"</p>
<p><br />
</p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<h2>usermod -l 修改登录名以后，如何修改用户组文件路径？</h2>
<p>ref : <a href="http://blog.csdn.net/lyc_daniel/article/details/12705373" target="_blank">
Linux中用户管理</a><br />
</p>
<p>&nbsp;</p>
<p><span style="font-size: 12px;">命令行键入：# usermod -d /home/newname -m newname<br />
</span></p>
<p><span style="font-size: 12px;"><img src="http://img.blog.csdn.net/20131015151801234" alt="" /><br />
</span></p>
<p><span style="font-size: 12px;">可以看出，/etc/passwd里的家目录部分已经修改成/home/<span style="font-size: 12px;">newname</span>。那/home下做了哪些修改呢？可以看出，原来的old_name文件改成了<span style="font-size: 12px;">newname</span>文件。这里需要说明一下usermod的<span style="font-family: Courier New;">-d</span>和<span style="font-family: Courier New;">-m</span>参数了：</span></p>
<p><span style="font-size: small;">如果命令是 <strong><span style="font-family: Courier New;"><br />
&nbsp;# usermod -d /home/<span style="font-size: 12px;">newname</span> <span style="font-size: 12px;">
newname</span></span></strong> <br />
表示仅修改 /etc/passwd 第6栏的内容而已；如果加上-m 参数，即命令 usermod -d /home/<span style="font-size: 12px;"><strong><span style="font-family: Courier New;"><span style="font-size: 12px;">newname</span></span></strong></span> -m
<span style="font-size: 12px;"><strong><span style="font-family: Courier New;"><span style="font-size: 12px;">newname</span></span></strong></span> ，则表示新建一个家目录；另外，如果原来的家目录是 /home/old_name，那么usermod -d /home/<span style="font-size: 12px;"><strong><span style="font-family: Courier New;"><span style="font-size: 12px;">newname</span></span></strong></span>
 -m <span style="font-size: 12px;"><strong><span style="font-family: Courier New;"><span style="font-size: 12px;">newname</span></span></strong></span> 命令会将原来的 /home/old_name 更名为 /home/<span style="font-size: 12px;"><strong><span style="font-family: Courier New;"><span style="font-size: 12px;">newname</span></span></strong></span></span></p>
<p>&nbsp;</p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<h2>在VNC Viewer, Virtual Machine 中移动窗口时，会自动发送Ctrl+C的现象：</h2>
<p>&nbsp;&nbsp;&nbsp; 一种可能是开了屏幕取词的工具，比如 有道词典、金山词霸 等。</p>
<p>vnc send Ctrl+C when moving window <br />
</p>
<p><br />
</p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<p>&nbsp;</p>
<h2>ssh: connect to host localhost/ubuntu port 22: Connection refused 解决方法</h2>
<p>
解决方法：<br />
&nbsp;$ sudo apt-get install openssh-server</p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<p>&nbsp;</p>
<h2>/etc/resolv.conf 被重写问题应对<br />
</h2>
<p>Linux配置DNS时出现/etc/resolv.conf被重写问题时，如果锁定之：</p>
<pre><span style="font-family: Courier New;">chattr +i /etc/resolv.conf</span></pre>
<pre>解锁 ：chattr +i /etc/resolv.conf
</pre>
<p><br />
</p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h2>Ubuntu gnome-terminal shortcut key: </h2>
<p>Ubuntu Terminal 快捷键：<br />
</p>
<pre><span style="font-family: Courier New;"> ubuntu Terminal add tab shortcut key:  Ctrl+Shift+T
                Move tab shortcut key:  Ctrl+PageDown/PageUp
               Swith tab shortcut key:&nbsp; Alt+(1-9): e.g.: Alt+1
               Close tab shortcut key:  Ctrl+Shift+W or Ctrl+D (Firefox=Ctrl+W)
	    close the entire terminal:  Ctrl + Shift + Q 

                     Move    workpace:  Ctrl+Alt+UP/Down/Left/Right
                    Move to workspace:  Ctrl+Alt+ 1/2/3/4..</span>
</pre>
<p><br />
</p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<p>
<span style="font-size: 24px; color: #ff00; line-height: 1.571428em;">Vim复制到系统剪贴板</span></p>
<div style="margin: 0px; padding: 0px; border: 0px; line-height: 1.571428em; font-family: gotham,helvetica,arial,sans-serif; font-size: 14px; color: #383838;">
<span style="font-size: 18px; line-height: 1.571428em;">1.首先查看vim --version |grep clipboard中 clipboard选项是否开启</span></div>
<div style="margin: 0px; padding: 0px; border: 0px; line-height: 1.571428em; font-family: gotham,helvetica,arial,sans-serif; font-size: 14px; color: #383838;">
<span style="font-size: 18px; line-height: 1.571428em;">2.ubuntu中通过下载vim-gnome可以开启系统剪贴板 sudo apt-get install vim-gnome</span></div>
<div style="margin: 0px; padding: 0px; border: 0px; line-height: 1.571428em; font-family: gotham,helvetica,arial,sans-serif; font-size: 14px; color: #383838;">
<span style="font-size: 18px; line-height: 1.571428em;">3.复制粘贴通过 "+y &nbsp;和 "+p 实现 （在一般模式下按v进入visual模式G全选后复制</span>）<br />
<strong>vim 命令：</strong><br />
</div>
<p><span style="background-color: #ff9900;">&lt;ESC&gt; <br />
ggVGy</span></p>
<p><span style="background-color: #ff9900;"><strong><span style="background-color: #ffffff;">说明：</span></strong><br />
</span></p>
<blockquote>
<p>&lt;ESC&gt; - 进入Normal模式<br />
gg -- 到文件开头<br />
V&nbsp; -- shift+v, 进入Visual-Line模式; v--进入visual单字模式<br />
G&nbsp; --&nbsp; shift + g, 到文件结尾<br />
y&nbsp;&nbsp; -- 复制到剪贴板；p - 粘贴<br />
</p>


</blockquote>
<p>=====================================<br />
</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h3>简单的Git 命令：获取仓库文件和切换远程分支<br />
</h3>
<p>&nbsp;</p>
<p><span style="background-color: #cccccc;"><span style="font-family: Courier New;">git clone git@gitlab.xxx.com:xxxxx.git</span></span>&nbsp; //从远程clone 分支下来，一般是默认master。</p>
<p>
有时不是，用 git branch 可以看到：</p>
<pre>* indigo-devel </pre>
<p>说明此时是在indigo-devel上。</p>
<h3>=== 要切换到master: ====</h3>
<p> <span style="background-color: #cccccc;"><span style="font-family: Courier New;">git branch -r</span></span> //查看远程分支,<br />
<span style="background-color: #cccccc;"><span style="font-family: Courier New;">git branch -a</span></span> //查看本地和远程分支。<br />
如： </p>
<pre>&nbsp;  remotes/origin/master
&nbsp;  remotes/origin/indigo-devel</pre>
<p>*如果本地没有master,要创建master：<br />
<span style="background-color: #cccccc;"><span style="font-family: Courier New;">git checkout -b master remotes/origin/master</span></span>&nbsp; //本地创建master分支，并与远程remotes/origin/master同步<br />
<span style="background-color: #cccccc;"><span style="font-family: Courier New;">git branch</span></span>&nbsp;&nbsp; //检查本地当前分支</p>
<h3>=== 切换到其他分支: ====</h3>
<p><span style="background-color: #cccccc;"><span style="font-family: Courier New;">git checkout hydro-devel</span></span>&nbsp; //从master 切换到 hydro-devel 分支<span style="font-family: Courier New;"><span style="background-color: #cccccc;"><br />
git checkout indigo-devel</span>&nbsp;</span> //从hydro 切换到 indigo-devel 分支<span style="background-color: #cccccc;"><span style="font-family: Courier New;"><br />
git checkout master</span></span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // 从indigo-devel 切换到 master 分支<br />
</p>
<p>
<br />
**有关Git，更多教程参见：<a href="http://edu.csdn.net/course/detail/1223" target="_blank">http://edu.csdn.net/course/detail/1223</a></p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<h3>&nbsp;</h3>
<h3>工业机器人运动规划算法包：openrave.org</h3>
<p>参考链接ref1:</p>
<p>参考链接ref2:<br />
</p>
<p>
正文： 工业机器人运动规划算法包：openrave.org</p>
<p>========== 参考格式 （新增记录时，复制粘贴在下）=============</p>
<h3><span style="font-size: 14px;">【日期】：<br />
</span></h3>
<h3><span style="font-size: 14px;">《<a href="http://blog.chinaunix.net/uid-24648266-id-5746385.html" target="_blank">.py运行python程序报错&ldquo;: No such file or directory&rdquo;的解决</a>》</span></h3>
<h3>&nbsp;</h3>
<p>参考链接ref1:</p>
<p>参考链接ref2:<br />
</p>
<p>
正文：</p>
<p><span style="font-size: 24px;">&nbsp;</span> </p>
<div class="Blog_con2">
<div class="Blog_con3">
<p>分类： <span>Python/Ruby</span></p>


</div>
<div class="Blog_wz1" style="word-wrap: break-word;">在linux上，执行python脚本，<br />
<span style="font-family: Consolas,monospace; font-size: 12px; line-height: 1.3; letter-spacing: 0.1px; background-color: #ffffff;"><br />
</span>
<pre name="code" class="python">    #!/bin/env python
    print 'hello'</pre>
<br />
用 <span style="font-family: Courier New;">python a.py</span> 的方式执行没有问题，<br />
用 <span style="font-family: Courier New;">./a.py</span> 的方式执行，报错&ldquo;: No such file or directory&rdquo;<br />
<br />
shell中直接执行&nbsp;env python 没问题，可以进入python解释器。<br />
<br />
修改a.py，不使用env，直接指定python的路径，<br />
<br />
<div id="codeText" class="codeText"><ol class="dp-css none_number" style="margin: 0 1px 0 0; padding: 5px 0pt;" start="1">
<li><span style="color: #000000;">#<span style="color: #0000cc;">!</span><span style="color: #0000cc;">/</span>usr<span style="color: #0000cc;">/</span>bin<span style="color: #0000cc;">/</span>python<br />
</span></li>
<li><span style="color: #0000ff;">print</span> <span style="color: #ff00ff;">'hello'</span></li>

</ol>
</div>


./a.py 执行，报错&ldquo;-bash: ./a.py: /usr/bin/python^M: bad interpreter: No such file or directory&rdquo;<br />
<br />
根据错误信息，发现文件是msdos格式的，行尾是\r\n。 run this comand:<br />
<pre>  $ sed -i 's#\r##' a.py</pre>
转换成unix格式后，问题解决。</div>
</div>
<p>=======================================================================</p>
<p>&nbsp;</p>
<h3>【日期】：2016年8月10日<br />
《Ubuntu更新源&nbsp; for x86》</h3>
<p>参考链接ref1:</p>
<p>参考链接ref2:<br />
</p>
<p>正文：</p>
<p>&nbsp;</p>
<h3 class="title pre fs1"><span class="tcnt">Ubuntu14.04 LTS更新源</span>&nbsp; for x86</h3>
<p>
<span style="line-height: 28px;">不同的网络状况连接以下源的速度不同, 建议在添加前手动验证以下源的连接速度(ping下就行),选择最快的源可以节省大批下载时间。</span></p>
<ul style="line-height: 28px;">
<li style="line-height: 28px;">首先备份源列表:</li>

</ul>
<pre>sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup
</pre>
<ul style="line-height: 28px;">
<li style="line-height: 28px;">而后用gedit或其他编辑器打开:</li>
</ul>
<pre><a style="line-height: 28px;" title="Qref Ubuntu Logo.png" href="http://wiki.ubuntu.org.cn/File:Qref_Ubuntu_Logo.png" rel="nofollow" target="_blank"><img style="line-height: 28px;" src="http://wiki.ubuntu.org.cn/images/thumb/1/13/Qref_Ubuntu_Logo.png/18px-Qref_Ubuntu_Logo.png" alt="" width="18" height="18" border="0" /></a> sudo gedit /etc/apt/sources.list
<a style="line-height: 28px;" title="Qref Kubuntu Logo.png" href="http://wiki.ubuntu.org.cn/File:Qref_Kubuntu_Logo.png" rel="nofollow" target="_blank"><img style="line-height: 28px;" src="http://wiki.ubuntu.org.cn/images/thumb/a/ab/Qref_Kubuntu_Logo.png/18px-Qref_Kubuntu_Logo.png" alt="" width="18" height="18" border="0" /></a> sudo kate /etc/apt/sources.list  
<a style="line-height: 28px;" title="Qref Xubuntu Logo.png" href="http://wiki.ubuntu.org.cn/File:Qref_Xubuntu_Logo.png" rel="nofollow" target="_blank"><img style="line-height: 28px;" src="http://wiki.ubuntu.org.cn/images/thumb/6/69/Qref_Xubuntu_Logo.png/18px-Qref_Xubuntu_Logo.png" alt="" width="18" height="18" border="0" /></a> sudo mousepad /etc/apt/sources.list
<a style="line-height: 28px;" title="Qref Xubuntu Logo.png" href="http://wiki.ubuntu.org.cn/File:Qref_Xubuntu_Logo.png" rel="nofollow" target="_blank"><img style="line-height: 28px;" src="http://wiki.ubuntu.org.cn/images/thumb/6/69/Qref_Xubuntu_Logo.png/18px-Qref_Xubuntu_Logo.png" alt="" width="18" height="18" border="0" /></a> sudo leafpad /etc/apt/sources.list (13.04版)
</pre>
<ul style="line-height: 28px;">
<li style="line-height: 28px;">从下面列表中选择合适的源，替换掉文件中所有的内容，保存编辑好的文件: <span style="font-family: Arial,sans-serif; line-height: 28px;"> <a style="line-height: 28px;" href="http://wiki.ubuntu.org.cn/Qref/Source" rel="nofollow" target="_blank">http://wiki.ubuntu.org.cn/Qref/Source</a></span><br />
</li>

</ul>
<dl style="line-height: 28px;"><dd style="line-height: 28px;"><a style="line-height: 28px;" title="Attention niels epting.svg" href="http://wiki.ubuntu.org.cn/File:Attention_niels_epting.svg" rel="nofollow" target="_blank"><img style="line-height: 28px;" src="http://wiki.ubuntu.org.cn/images/thumb/5/51/Attention_niels_epting.svg/18px-Attention_niels_epting.svg.png" alt="" width="18" height="18" border="0" /></a>&nbsp;<span style="color: red; line-height: 28px;">注意：一定要选对版本</span></dd></dl>
<ul style="line-height: 28px;">
<li style="line-height: 28px;">然后，刷新列表:<span style="line-height: 28px;">&nbsp;&nbsp; </span><a style="line-height: 28px;" title="Attention niels epting.svg" href="http://wiki.ubuntu.org.cn/File:Attention_niels_epting.svg" rel="nofollow" target="_blank"><img style="line-height: 28px;" src="http://wiki.ubuntu.org.cn/images/thumb/5/51/Attention_niels_epting.svg/18px-Attention_niels_epting.svg.png" alt="" width="18" height="18" border="0" /></a>&nbsp;<span style="color: red; line-height: 28px;">注意：一定要执行刷新</span></li>

</ul>
<pre>sudo apt-get update
</pre>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>=======================================================</p>
<h3>【日期】：2016年8月10日<br />
《linux挂载U盘》</h3>
<p>参考链接ref1:</p>
<p>参考链接ref2:<br />
</p>
<p>
正文：</p>
<p>有时候只有Ubuntu server，有时候Ubuntu Desktop不能自动挂载U盘。这个时候需要一些命令：<br />
<strong>1.在插入U盘前和插入U盘后，都输入同一个命令，检查多了哪个盘</strong><br />
&nbsp; cat /proc/partitions<br />
&nbsp; 这里我发现多了<br />
</p>
<pre>&nbsp;&nbsp; 8&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 16&nbsp;&nbsp;&nbsp; 7827424 sdb
&nbsp;&nbsp; 8&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 17&nbsp;&nbsp;&nbsp; 7825423 sdb1</pre>
<p><br />
&nbsp; sdb是统称，所以新插入的U盘就是<span style="font-family: Courier New;">/dev/sdb1</span><br />
<strong>2.用命令检查新的U盘的文件系统格式</strong><br />
</p>
<pre>&nbsp; $ sudo fdisk -l /dev/sdb</pre>
<p><br />
&nbsp; Disk /dev/sdb: 8015 MB, 8015282176 bytes<br />
&nbsp; 247 heads, 62 sectors/track, 1022 cylinders, total 15654848 sectors<br />
&nbsp; Units = sectors of 1 * 512 = 512 bytes<br />
&nbsp; Sector size (logical/physical): 512 bytes / 512 bytes<br />
&nbsp; I/O size (minimum/optimal): 512 bytes / 512 bytes<br />
&nbsp; Disk identifier: 0x0001fce0<br />
<br />
&nbsp;&nbsp; Device Boot&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Start&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; End&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Blocks&nbsp;&nbsp; Id&nbsp; System<br />
/dev/sdb1&nbsp;&nbsp; *&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 62&nbsp;&nbsp;&nbsp; 15650907&nbsp;&nbsp;&nbsp;&nbsp; 7825423&nbsp;&nbsp;&nbsp; c&nbsp; W95 FAT32 (LBA)<br />
<br />
&nbsp; 看到这里是FAT32格式。<br />
<br />
<strong>3.mount </strong><br />
&nbsp; sudo mkdir /media/udisk<br />
&nbsp; mount -t vfat /dev/sdb1 /media/udisk<br />
<br />
&nbsp; 注意: &nbsp;<br />
&nbsp; mount -t 按两次tab键会提示输入什么文件系统类型<br />
&nbsp; /media/udisk是我自己创建的目录<br />
<br />
<strong>4.umount</strong><br />
&nbsp; umount /media/udisk<br />
</p>
<p>=======================================================</p>
<p>&nbsp;</p>
<h3>【日期】：2016年7月25日<br />
《通过Terminal 、SSH 配置ubuntu连接wifi 网络 》</h3>
<p><br />
</p>
<p>参考链接ref1:&nbsp; <a href="http://askubuntu.com/questions/522842/ubuntu-14-04-connect-to-a-wifi-network-using-command-line" target="_blank">
http://askubuntu.com/questions/522842/ubuntu-14-04-connect-to-a-wifi-network-using-command-line</a><br />
</p>
<p>参考链接ref2: <a href="http://askubuntu.com/questions/16584/how-to-connect-and-disconnect-to-a-network-manually-in-terminal" target="_blank">
http://askubuntu.com/questions/16584/how-to-connect-and-disconnect-to-a-network-manually-in-terminal</a><br />
</p>
<p>正文： Connecting to wifi via terminal/ssh/uart for Ubuntu/Lubuntu<br />
Show available wlan access points:<br />
$ nmcli dev wifi<br />
<br />
Connect with access point:<br />
$ nmcli dev wifi connect $ACCESS_POINT password $PASSWORD<br />
</p>
<p><br />
</p>
<p>=======================================================</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h3>【日期】： 20160525<br />
《Ubuntu Linux 使用L2TP客户端配置》</h3>
<p>&nbsp;</p>
<p>参考链接ref1: <a href="https://www.yuntidata.com/guides/ubuntu_l2tp" target="_blank">
https://www.yuntidata.com/guides/ubuntu_l2tp</a><br />
</p>
<p>参考链接ref2:<br />
</p>
<p>
正文：============================<br />
Ubuntu Linux 使用L2TP客户端配置<br />
1. 安装 l2tp-ipsec-vpn<br />
&nbsp;&nbsp;&nbsp; 在 terminal 中执行以下命令，<br />
&nbsp;&nbsp;&nbsp; sudo apt-get install l2tp-ipsec-vpn<br />
&nbsp;&nbsp;&nbsp; 当遇到这个对话框时（Use an X.509 certificate for this host?），选择"No"<br />
&nbsp;&nbsp;&nbsp; 安装完成重启电脑：<br />
&nbsp;&nbsp;&nbsp; sudo shutdown -r now<br />
<br />
2. 配置 VPN<br />
&nbsp;&nbsp;&nbsp; 右键点击屏幕右上角多出的 l2tp-ipsec-vpn 标志，在下拉选项里点击"Edit Connections"进入 VPN 的设置（可能遇到屏闪，使用键盘操作）<br />
<br />
3. 添加 VPN<br />
&nbsp;&nbsp;&nbsp; 点击右侧的"Add"按钮，"Connection name"栏中填入"VPN_L2TP"<br />
<br />
4. 编辑 VPNCloud_L2TP<br />
&nbsp;&nbsp;&nbsp; 选中列表中的"VPN_L2TP"，点击右侧的"Edit"按钮<br />
<br />
5. 填写 VPN 服务器和域名<br />
&nbsp;&nbsp;&nbsp; IPsec 标签下，"Remote Server"中填入服务器域名（前往服务器列表页面查看）；<br />
&nbsp;&nbsp;&nbsp; &ldquo;Use pre-shared key for authentication&rdquo;中填入具体密钥<br />
<br />
6. 设置 VPN 协议及填写帐号信息<br />
&nbsp;&nbsp;&nbsp; 点击 PPP 标签，选中"Allow these protocols"，并只勾选列表中的"Microsoft CHAP Version 2 (MS-CHAPv2)"；<br />
&nbsp;&nbsp;&nbsp; "User name"中填入你的VPN帐号用户名；"Password"中填入你的VPN帐号密码；最后点击"IP settings..."按钮<br />
<br />
7. IP 设置<br />
&nbsp;&nbsp;&nbsp; 勾选"Obtain DNS server addresses automatically"，点击"OK"按钮<br />
<br />
8. 关闭窗口，让设置生效<br />
&nbsp;&nbsp;&nbsp; 一定要点击"Close"按钮，并点击弹出框内的"OK"按钮，设置才会生效<br />
<br />
9. 连接 VPN<br />
&nbsp;&nbsp;&nbsp; 右键点击 l2tp-ipsec-vpn 标志，点击"VPNCloud_L2TP"连接 VPN；如果连接不上，可以重启 l2tp-ipsec-vpn 或者重启系统后再连接<br />
<br />
10.连接成功<br />
&nbsp;&nbsp;&nbsp; VPN 连接成功后 l2tp-ipsec-vpn 标志上就没有红叉；右键点击标志可进行 Disconnect 等操作</p>
<p>=======================================================</p>
<h3>【日期】：2016年4月26日14:02:34<br />
《linux下的find文件查找命令与grep文件内容查找命令》</h3>
<p>参考链接ref1: <a href="http://www.cnblogs.com/zhangmo/p/3571735.html" target="_blank">
http://www.cnblogs.com/zhangmo/p/3571735.html</a></p>
<p>参考链接ref2:<br />
</p>
<p>正文：&nbsp;</p>
<p>linux下的find文件查找命令与grep文件内容查找命令<br />
<br />
　　在使用linux时，经常需要进行文件查找。其中查找的命令主要有find和grep。两个命令是有区的。<br />
<br />
　　区别：(1)find命令是根据文件的属性进行查找，如文件名，文件大小，所有者，所属组，是否为空，访问时间，修改时间等。 <br />
<br />
(2)grep是根据文件的内容进行查找，会对文件的每一行按照给定的模式(patter)进行匹配查找。<br />
<br />
　　<strong>一.find命令</strong><br />
<br />
　　　　基本格式：find path expression<br />
<br />
　　　　1.按照文件名查找<br />
<br />
　　　　(1)find / -name httpd.conf　　#在根目录下查找文件httpd.conf，表示在整个硬盘查找<br />
　　　　(2)find /etc -name httpd.conf　　#<span style="color: #cc0000;">在/etc目录下文件httpd.conf</span><br />
　　　　(3)find /etc -name '*srm*'　　#使用通配符*(0或者任意多个)。表示在/etc目录下查找文件名中含有字符串&lsquo;srm&rsquo;的文件<br />
　　　　(4)find . -name 'srm*' 　　#表示当前目录下查找文件名开头是字符串&lsquo;srm&rsquo;的文件<br />
<br />
　　　　2.按照文件特征查找 　　　　<br />
<br />
　　　　(1)find / -amin -10 　　# 查找在系统中最后10分钟访问的文件(access time)<br />
　　　　(2)find / -atime -2　　 # 查找在系统中最后48小时访问的文件<br />
　　　　(3)find / -empty 　　# 查找在系统中为空的文件或者文件夹<br />
　　　　(4)find / -group cat 　　# 查找在系统中属于 group为cat的文件<br />
　　　　(5)find / -mmin -5 　　# 查找在系统中最后5分钟里修改过的文件(modify time)<br />
　　　　(6)find / -mtime -1 　　#查找在系统中最后24小时里修改过的文件<br />
　　　　(7)find / -user fred 　　#查找在系统中属于fred这个用户的文件<br />
　　　　(8)find / -size +10000c　　#查找出大于10000000字节的文件(c:字节，w:双字，k:KB，M:MB，G:GB)<br />
　　　　(9)find / -size -1000k 　　#查找出小于1000KB的文件<br />
<br />
　　　　3.使用混合查找方式查找文件<br />
<br />
　　　　参数有： ！，-and(-a)，-or(-o)。<br />
<br />
　　　　(1)find /tmp -size +10000c -and -mtime +2 　　#在/tmp目录下查找大于10000字节并在最后2分钟内修改的文件<br />
　　 (2)find / -user fred -or -user george 　　#在/目录下查找用户是fred或者george的文件文件<br />
　　 (3)find /tmp ! -user panda　　#在/tmp目录中查找所有不属于panda用户的文件<br />
　　 <br />
<br />
　　<strong>二、grep命令</strong><br />
<br />
　　　 基本格式：find expression<br />
<br />
　　　 1.主要参数<br />
<br />
　　　　[options]主要参数：<br />
　　　　－c：只输出匹配行的计数。<br />
　　　　－i：不区分大小写<br />
　　　　－h：查询多文件时不显示文件名。<br />
　　　　－l：查询多文件时只输出包含匹配字符的文件名。<br />
　　　　－n：显示匹配行及行号。<br />
　　　　－s：不显示不存在或无匹配文本的错误信息。<br />
　　　　－v：显示不包含匹配文本的所有行。<br />
<br />
　　　　pattern正则表达式主要参数：<br />
　　　　\： 忽略正则表达式中特殊字符的原有含义。<br />
　　　　^：匹配正则表达式的开始行。<br />
　　　　$: 匹配正则表达式的结束行。<br />
　　　　\&lt;：从匹配正则表达 式的行开始。<br />
　　　　\&gt;：到匹配正则表达式的行结束。<br />
　　　　[ ]：单个字符，如[A]即A符合要求 。<br />
　　　　[ - ]：范围，如[A-Z]，即A、B、C一直到Z都符合要求 。<br />
　　　　.：所有的单个字符。<br />
　　　　* ：有字符，长度可以为0。<br />
<br />
　　　　2.实例　 <br />
<br />
　　(1)grep 'test' d*　　#显示所有以d开头的文件中包含 test的行<br />
　　(2)grep &lsquo;test&rsquo; aa bb cc 　　 #显示在aa，bb，cc文件中包含test的行<br />
　　(3)grep &lsquo;[a-z]\{5\}&rsquo; aa 　　#显示所有包含每行字符串至少有5个连续小写字符的字符串的行<br />
　　(4)grep magic /usr/src　　#显示/usr/src目录下的文件(不含子目录)包含magic的行<br />
　　(5)grep -r magic /usr/src　　#<span style="color: #cc0000;">显示/usr/src目录下的文件(包含子目录)包含magic的行</span><br />
<br />
　　(6)grep -w pattern files ：只匹配整个单词，而不是字符串的一部分(如匹配&rsquo;magic&rsquo;，而不是&rsquo;magical&rsquo;)，<br />
</p>
<p>=========================================================</p>
<h3>【日期】：2016年2月2日11:15:59<br />
《/dev/ttyUSB0 permission denied　解决办法：永久有串口可操作权限》</h3>
<p>参考链接ref1:</p>
<p>参考链接ref2:<br />
</p>
<p>正文： <br />
Problem: /dev/ttyUSB0 permission denied.<br />
You're not root, has no permission. <br />
Generally, do this way:<br />
</p>
<pre>　　　$ sudo chmod 777 /dev/ttyUSB0</pre>
<p>This isn&rsquo;t very safe, strictly speaking, nor is it permanent. The change will need to be repeated every time you reboot the computer or unplug and reconnect the device.<br />
</p>
<pre>  　　$ sudo usermod -aG dialout &lt;username&gt;</pre>
<p>&lt;username&gt; is your username. Add this user into your dialout userGroup. Log out your linux and you can access your device permanently.<br />
</p>
<p>=========================================================</p>
<p>&nbsp;</p>
<h3>&nbsp;</h3>
<h3>【日期】：2016年1月11日11:23:45<br />
《关于Ubuntu的软件源管理，repositories，PPA等问题》</h3>
<p>&nbsp;</p>
<p>参考链接ref1:</p>
<p>参考链接ref2:<br />
</p>
<p>正文：<a href="https://help.ubuntu.com/community/Repositories/Ubuntu" target="_blank">https://help.ubuntu.com/community/Repositories/Ubuntu</a></p>
<p>&nbsp; <br />
</p>
<p>=========================================================</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<h3>【日期】：20160106<br />
《备份ubuntu系统为iso文件，可用于直接安装》</h3>
<p>&nbsp;</p>
<p>参考链接ref1:</p>
<p>参考链接ref2:<br />
</p>
<p>
正文：</p>
<p>&nbsp;&nbsp;&nbsp; 已经配置好的ubuntu系统（12.04 or newer）, 希望做成iso文件可以供下面的使用者直接安装，请使用工具remastersys.<br />
&nbsp;&nbsp;&nbsp; 使用remastersys， 需要先添加ppa源,参考：<a href="https://launchpad.net/~mutse-young/+archive/ubuntu/remastersys" target="_blank">https://launchpad.net/~mutse-young/+archive/ubuntu/remastersys</a><br />
&nbsp;&nbsp;&nbsp; 然后使用命令：</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &gt;&gt; sudo add-apt-repository ppa:mutse-young/remastersys <br />
</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &gt;&gt; sudo apt-get update</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &gt;&gt; sudo apt-get insstall remastersys</p>
<p>&nbsp;&nbsp;&nbsp; 上述过程期间可能会遇到一些错误，翻墙以后多试几次就可以。至于怎么使用，先用建立dist:<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &gt;&gt; sudo remastersys dist cdfs</p>
<p>&nbsp;&nbsp;&nbsp; 再创建iso：<br />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &gt;&gt; sudo remastersys backup iso my_ubuntu_12.04_backup.iso</p>
<p>&nbsp;&nbsp;&nbsp; 即可完成。你将得到如下信息：</p>
<pre>Creating the iso file only
System Backup Mode Selected
Making disk compatible with Ubuntu Startup Disk Creator.
Creating md5sum.txt for the livecd/dvd
Creating my_ubuntu_12.04_backup.iso in /home/remastersys/remastersys
Creating my_ubuntu_12.04_backup.iso.md5 in /home/remastersys/remastersys
/home/remastersys/remastersys/my_ubuntu_12.04_backup.iso which is 2.2G in size is ready to be burned or tested in a virtual machine.
</pre>
<h3>【日期】：<br />
《查找linux程序进程号-杀死进程》</h3>
<p>&nbsp;</p>
<p>linux下另外一个ps命令查找与进程相关的PID号：ps aux | grep program_filter_word<br />
查找到进程ID以后可使用kill命令杀死进程。<br />
参考链接ref1:http://os.51cto.com/art/200905/125605.htm<br />
参考链接ref2:</p>
<p>=========================================================</p>
<h3>【日期】：<br />
《Linux下可以监听串口上数据的应用》</h3>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>
ref:<a href="http://unix.stackexchange.com/questions/12359/how-can-i-monitor-serial-port-traffic" target="_blank">http://unix.stackexchange.com/questions/12359/how-can-i-monitor-serial-port-traffic</a><br />
Serial Port Traffic/Data Monitor for Linux<br />
Linux下可以监听串口上数据的应用<br />
<br />
There is port monitoring tool to watch the packets written on the port. Especially when you want to check if your program written works. Tool to see if your application is sending the messages to the port.<br />
</p>
<p>=========================================================</p>
<p>&nbsp;</p>
<h3>【20141104】：《Ubuntu Linux下设置IP的配置命令》<br />
</h3>
<p>参考链接ref1: &nbsp;&nbsp;<a href="http://www.cnblogs.com/empire/archive/2011/01/10/1931877.html" target="_blank">http://www.cnblogs.com/empire/archive/2011/01/10/1931877.html</a></p>
<p><span style="font-family: 'Courier New'; font-size: 14px;">参考链接ref2:</span><br />
</p>
<p>=========================================================</p>
<div><br />
</div>
<h3>【20141104】：Ubuntu 12.04 中自定义DNS服务器设置</h3>
<p><span style="font-family: Courier New; font-size: 14px;"><a href="http://blog.renhao.org/2012/05/ubuntu-12-04-add-dns-nameservers/" target="_blank">http://blog.renhao.org/2012/05/ubuntu-12-04-add-dns-nameservers/</a><br />
</span></p>
<p><span style="font-family: Courier New; font-size: 14px;"><a href="http://askubuntu.com/questions/130452/how-do-i-add-a-dns-server-via-resolv-conf" target="_blank">http://askubuntu.com/questions/130452/how-do-i-add-a-dns-server-via-resolv-conf</a></span></p>
<p><br />
</p>
<p>==================================================================</p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
<p>
<span style="font-family: Courier New; font-size: 14px;">&nbsp; &nbsp; ====== Linux Freq-Used Commands and Accounts Management ======<br />
<br />
<br />
(I) &nbsp;Linux Commands<br />
(1.1) Linux Commands Format<br />
(1.2) Get Commands' Help<br />
<br />
<br />
(II) Frequent used Linux Commands<br />
(2.1) File and Folder Operation Commands<br />
(2.2) Use DVD Disk and U Disk<br />
<br />
<br />
(III)RPM Package Management<br />
(3.1) Package Manage System<br />
(3.2) RPM Package Manage System<br />
<br />
<br />
(IV) User and Group Manage Commands<br />
(4.1) User Management<br />
(4.2) Group Management<br />
(4.3) File Authority Setting<br />
</span></p>
<p><span style="font-family: Courier New; font-size: 14px;"><br />
</span></p>
<p><span style="font-family: Courier New; font-size: 14px;"><br />
</span></p>
<p><br />
</p>
<p>&nbsp;</p>
<p></p>
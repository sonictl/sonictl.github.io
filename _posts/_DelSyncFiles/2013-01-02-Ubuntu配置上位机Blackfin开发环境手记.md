---
title: 'Ubuntu配置上位机Blackfin开发环境手记'
date: 2013-01-02 21:37:00
---


<p>-------- 本文档适合使用Ctrl&#43;F 搜索关键字 --------------</p>
<p>-------- It‘s more convenient to this message by searching key word with Ctrl&#43;F -------</p>
<p style="text-align:center"><span style="color:rgb(70,70,70); font-size:14px; line-height:21px"><span style="font-family:Arial"><strong>My note for configuring the Software Development Environment(SDE) for TLL board on Ubuntu 12.04</strong></span></span><br>
</p>
<p style="text-align:center"><span style="font-family:Microsoft YaHei; font-size:24px">Ubuntu配置上位机Blackfin开发环境手记：</span></p>
<p>[参考]<a href="http://blog.csdn.net/cp1300/article/details/8205282" target="_blank"><span style="color:#336699">http://blog.csdn.net/cp1300/article/details/8205282</span></a></p>
<p><span style="color:#464646">&nbsp;</span></p>
<p>【Section 0: Working Aim&nbsp;工作目的】</p>
<p>现在手里有一个嵌入式开发板，搭载的是&nbsp;AnalogDevices&nbsp;公司的&nbsp;Blackfin527 Processor，预装板载Uclinux操作系统。现在要配置上位机Linux/Ubuntu12.04&nbsp;的开发环境，包括BlackfinToolchain，Eclipse开发环境，和Library.</p>
<p>&nbsp;</p>
<p>【Section1: Install Toolchain】</p>
<p><strong><span style="color:#111111">Install Blackfin Toolchain</span></strong></p>
<p><span style="color:#464646">Refer to the Note after this section if you meet some issueduring this installation.</span></p>
<p><strong><span style="color:#111111">1.1 Package Installation (recommended)</span></strong></p>
<p><strong>l&nbsp; Get the latest Blackfin Toolchain from&nbsp;<a href="http://blackfin.uclinux.org/gf/project/toolchain/frs/"><span style="color:windowtext">ADI</span></a></strong></p>
<p><strong>l&nbsp; Current up to date version is: 2011R1-RC4</strong></p>
<p><strong>l&nbsp; Example download command:</strong></p>
<div style="background:#EEECE1">
<p>&gt; wget http://blackfin.uclinux.org/gf/download/frsrelease/531/9508/blackfin-toolchain-2011R1-RC4.i386.rpm</p>
<p>&gt; wget<a href="http://blackfin.uclinux.org/gf/download/frsrelease/531/9512/blackfin-toolchain-elf-gcc-4.3-2011R1-RC4.i386.rpm"><span style="color:windowtext">http://blackfin.uclinux.org/gf/download/frsrelease/531/9512/blackfin-toolchain-elf-gcc-4.3-2011R1-RC4.i386.rpm</span></a></p>
</div>
<p><span style="color:#111111">·</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#111111">Check if a previous toolchain is already installed</span></p>
<p><span style="color:#111111">·</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#111111">If so, remove old tool chain using rpm -e ...</span></p>
<div style="background:#EEECE1">
<p>&gt; rpm -qa blackfin-toolchain\*</p>
</div>
<p><span style="color:#111111">·</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#111111">Install Toolchain using RPM.</span></p>
<p><span style="color:#111111">·</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#111111">Note: If a previous toolchain is installed, you may need touninstall the previous toolchain see above</span></p>
<div style="background:#EEECE1">
<p>rpm-ivh blackfin-toolchain-*</p>
</div>
<p><span style="color:#111111">More details on toolchain installation can be found atADI's&nbsp;<a href="http://docs.blackfin.uclinux.org/doku.php?id=toolchain:installing"><span style="color:#3E73A0">installation wiki entry.</span></a></span></p>
<p><strong><span style="color:#111111">1.2 Source Installation (advanced)</span></strong></p>
<p><span style="color:#111111">·</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#111111">Prequisites</span></p>
<div style="background:#EEECE1">
<p>yuminstall automake autoconf ncurses-devel zlib-devel texinfo</p>
</div>
<p><span style="color:#111111">·</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#111111">set desired version</span></p>
<div style="background:#EEECE1">
<p>setVERSION=2011R1-RC2</p>
</div>
<p><span style="color:#111111">·</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#111111">Get the latest Version of the GNU toolchain</span></p>
<div style="background:#EEECE1">
<p>svncheckout svn://sources.blackfin.uclinux.org/toolchain/tags/$VERSION/</p>
</div>
<p><span style="color:#111111">·</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#111111">Compile and Install</span></p>
<div style="background:#EEECE1">
<p>&gt;cd bfin_toolchain.$VERSION/buildscript/ &gt; ./BuildToolChain -o&lt;COMPILER_NSTALLATION_DIR&gt;</p>
</div>
<p><span style="color:#464646">Note:</span>&nbsp;<span style="color:#464646">这里要注意的是：</span></p>
<p><span style="color:#464646">1.ADIoffers serious packages of toolchain of one version for different usages. Youshould select which one is needed.&nbsp; I downloaded the 3 packages:</span></p>
<div style="background:#EEECE1">
<p>&nbsp;&gt; blackfin-toolchain-2012R2-RC1.i386.rpm</p>
<p>&nbsp;&gt; blackfin-toolchain-elf-gcc-4.3-2012R2-RC1.i386.rpm</p>
<p>&nbsp;&gt; blackfin-toolchain-uclibc-default-2012R2-RC1.i386.rpm</p>
</div>
<p><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;from the Blackfin Linuxwebsite in the toolchain file release page</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">2. Ubuntu does not support installing this toolchain by RPMtool.</span></p>
<p><span style="color:#111111">&nbsp; &nbsp;&nbsp;I used the &quot;How to Install rpm package onUbuntu&quot; approach to go around it. Chinese tutorial:</span></p>
<p><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#111111">注意，用</span><span style="color:#111111">alien</span><span style="color:#111111">转换的</span><span style="color:#111111">deb</span><span style="color:#111111">包并不能保证</span><span style="color:#111111">100%</span><span style="color:#111111">顺利安装，所以可以找到</span><span style="color:#111111">deb</span><span style="color:#111111">最好直接用</span><span style="color:#111111">deb</span></p>
<p><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#111111">有时候，我们想要使用的软件并没有被包含到</span>&nbsp;<span style="color:#111111">Ubuntu</span>&nbsp;<span style="color:#111111">的仓库中，而程序本身也没有提供让</span>&nbsp;<span style="color:#111111">Ubuntu</span><span style="color:#111111">可以使用的</span>&nbsp;<span style="color:#111111">deb</span>&nbsp;<span style="color:#111111">包，你又不愿从源代码编译。但假如软件提供有</span>&nbsp;<span style="color:#111111">rpm</span>&nbsp;<span style="color:#111111">包的话，我们也是可以在</span>&nbsp;<span style="color:#111111">Ubuntu</span><span style="color:#111111">中安装的。</span></p>
<p><span style="color:#111111">方法一：</span></p>
<p><span style="color:#111111">1.</span>&nbsp;<span style="color:#111111">先安装</span>&nbsp;<span style="color:#111111">alien</span>&nbsp;<span style="color:#111111">和</span>&nbsp;<span style="color:#111111">fakeroot</span>&nbsp;<span style="color:#111111">这两个工具，其中前者可以将</span>&nbsp;<span style="color:#111111">rpm</span>&nbsp;<span style="color:#111111">包转换为</span>&nbsp;<span style="color:#111111">deb</span>&nbsp;<span style="color:#111111">包。安装命令为：</span>&nbsp;<span style="color:#111111">Intall
 the &quot;alien&quot;and &quot;fakeroot&quot; tools:</span><span style="color:#111111"><br>
</span><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></p>
<p>$&nbsp;<span style="color:#464646">sudo apt-get install alien fakeroot</span></p>
<p><span style="color:#111111">2.</span>&nbsp;<span style="color:#111111">将需要安装的</span>&nbsp;<span style="color:#111111">rpm</span>&nbsp;<span style="color:#111111">包下载备用，假设为</span>&nbsp;<span style="color:#111111">package.rpm</span><span style="color:#111111">。</span>&nbsp;<span style="color:#111111">Assume
 your package is:package.rpm</span><span style="color:#111111"><br>
</span><span style="color:#464646"><br>
</span><span style="color:#464646">3.</span>&nbsp;<span style="color:#464646">使用</span>&nbsp;<span style="color:#464646">alien</span>&nbsp;<span style="color:#464646">将</span>&nbsp;<span style="color:#464646">rpm</span>&nbsp;<span style="color:#464646">包转换为</span>&nbsp;<span style="color:#464646">deb</span>&nbsp;<span style="color:#464646">包：</span>&nbsp;<span style="color:#464646">use
 alien to convert rpmpackage into deb package:</span></p>
<p><span style="color:#464646">$&nbsp;fakeroot alien package.rpm</span></p>
<p><span style="color:#111111">4.</span>&nbsp;<span style="color:#111111">一旦转换成功，我们可以即刻使用以下指令来安装：</span>&nbsp;<span style="color:#111111">After a while, if succeed, install it using command:</span></p>
<p><span style="color:#464646">$ sudo dpkg -i package.deb</span></p>
<p><span style="color:#111111">如何转换</span><span style="color:#111111">rpm</span><span style="color:#111111">详细教程，参考这里</span><span style="color:#111111">:&nbsp;<a href="http://zhidao.baidu.com/question/377062512"><span style="color:#336699">http://zhidao.baidu.com/question/377062512</span></a></span></p>
<p><strong><span style="color:#111111">[Section2. Install Eclipse]</span></strong></p>
<p><span style="color:#111111">Follow OS independent instructions:</span><span style="color:#111111"><br>
</span><span style="color:#111111">Generally, follow instructions at:&nbsp;<a href="http://docs.blackfin.uclinux.org/doku.php?id=toolchain:eclipse:install"><span style="color:#3E73A0">link</span></a></span></p>
<p><span style="color:#111111">Note: It seems that Blackfin Toolchain only works in HeliosEclipse</span></p>
<p><span style="color:#111111">Install components:&nbsp;</span></p>
<p><span style="color:#111111">•</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp; Blackfin ucLinux</span></p>
<p><span style="color:#111111">•</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp; GDB Hardware Debugging</span></p>
<p><span style="color:#111111">•</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp; Target Terminal Management</span></p>
<p><span style="color:#111111">•</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp; ZynxCDT</span></p>
<p><span style="color:#111111">Below is my steps:</span></p>
<p><span style="color:#111111">Refer to this web page:http://docs.blackfin.uclinux.org/doku.php?id=toolchain:eclipse:install</span></p>
<p><strong><span style="color:#464646">&nbsp;&nbsp;&nbsp;2.1.Install the JavaRuntime Environment (JRE)</span></strong></p>
<p><span style="color:#464646">Checking your Java Runtime Environment version:&nbsp;</span></p>
<div style="background:#EEECE1">
<p>$java -version</p>
</div>
<p><span style="color:#464646">If you do not have Java installed, go through the :<a href="http://www.64bitjungle.com/ubuntu/install-java-jre-160-update-x-on-hardy-as-the-default-java-runtime/"><span style="color:#336699">Install Java Runtime Env. 1.6.0</span></a></span><span style="color:#464646">&nbsp;</span></p>
<p><span style="color:#464646">After you finished, the lines below should be displayed:</span></p>
<div style="background:#EEECE1">
<p>$ java -version<br>
java version &quot;1.6.0_16&quot;<br>
Java(TM) SE Runtime Environment (build 1.6.0_16-b01)<br>
Java HotSpot(TM) Client VM (build 14.2-b01, mixed mode, sharing)</p>
</div>
<p><strong><span style="color:#464646">&nbsp;</span></strong></p>
<p><strong><span style="color:#464646">&nbsp;2.2 Install the IDDE (Eclipse)</span></strong></p>
<p><span style="color:#464646">There are no executable installers for Eclipse. It isdistributed as a zip archive which you can unpack anywhere you like, and thenjust run the&nbsp;eclipse.exe&nbsp;program in the eclipse sub-directory.</span></p>
<p><span style="color:#464646">To get the installer, visit&nbsp;the&nbsp;<a href="http://www.eclipse.org/downloads/" target="_blank"><span style="color:#336699">Eclipse download page.</span></a></span></p>
<p><span style="color:#464646">You will be presented with a variety of download choices. Theonly difference between them is the default plug-in set. Since you will mostlikely be compiling/debugging code for the Blackfin processor, you shouldpick&nbsp;Eclipse&nbsp;IDE&nbsp;for
 C/C&#43;&#43; Developers.:&nbsp;</span></p>
<p></p>
<p><span style="color:#464646">&nbsp;Note: It seems that Blackfin Toolchain only works in&nbsp;<a href="http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/helios/SR2/eclipse-cpp-helios-SR2-linux-gtk.tar.gz" target="_blank"><span style="color:#336699">Helios
 Version</span></a>&nbsp;(Downloadthis verion)</span></p>
<p><span style="color:#464646">Extract: eclipse-cpp-helios-SR2-linux-gtk.tar.gz ,&nbsp;</span><span style="color:#464646"><br>
</span><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.Double-Clickon Archive and Extract Eclipse into /tmp</span></p>
<p><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.Open&nbsp;Terminal&nbsp;Window</span></p>
<p><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.Install RequiredPackages</span></p>
<p><span style="color:#464646">&nbsp;</span></p>
<div style="background:#EEECE1">
<p>$&nbsp;sudo su<br>
# apt-get install g&#43;&#43;</p>
</div>
<p><span style="color:#464646">&nbsp;</span></p>
<p><span style="color:#464646">&nbsp;4.Relocate Eclipse</span></p>
<div style="background:#EEECE1">
<p>#&nbsp;mv/tmp/eclipse /opt</p>
</div>
<p><span style="color:#464646">&nbsp;5.Create a Symlink</span></p>
<div style="background:#EEECE1">
<p>#&nbsp;ln-s /opt/eclipse/eclipse /usr/bin/eclipse</p>
</div>
<p><span style="color:#464646">6. After, you can Start Eclipse from Terminal simply with:</span></p>
<div style="background:#EEECE1">
<p>#&nbsp;eclipse</p>
</div>
<p><span style="color:#464646">or: (the way by which we launch Eclipse is under the Super Userpermision)</span></p>
<div style="background:#EEECE1">
<p>$exit<br>
$ sudo eclipse</p>
</div>
<p><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If you want toovercome the root permission issue, refer to:&nbsp;<a href="http://jonathan.lalou.free.fr/?p=1310" target="_blank"><span style="color:#3E73A0">here</span></a>.&nbsp;but it does not work on my Ubuntu.</span></p>
<p><span style="color:#464646">&nbsp;</span></p>
<p><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Reference:&nbsp;<a href="http://install-climber.blogspot.com/2012/10/installEclipseJunoCPPUbuntu1210Quantal.html" target="_blank"><span style="color:#3E73A0">How-to Install Eclipse Juno4.2 C/C&#43;&#43; on Ubuntu</span></a></span><span style="color:#464646"><br>
</span><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IfInstalled Eclipse on Linux, but it does not start,</span><span style="color:#464646"><a href="http://wiki.eclipse.org/IRC_FAQ#I_just_installed_Eclipse_on_Linux.2C_but_it_does_not_start._What_is_the_problem.3F" title="IRC FAQ"><span style="color:#3E73A0">read
 this</span></a></span><span style="color:#464646"><br>
</span><span style="color:#464646"><br>
</span><strong><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;2.3 Install the&nbsp;Blackfin Plug-ins</span></strong></p>
<p><span style="color:#464646">&nbsp;</span><span style="color:#464646">The Blackfin plug-ins willrequire Eclipse 4.2, as well as the CDT. Additionally, you will have to installthe GDB Hardware Debugging CDT plug-in (it is usually not installed bydefault).&nbsp;</span><span style="color:#464646"><br>
</span><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Toinstall the GDB Hardware Debugging plug-in, the CDT update site needsenabled.&nbsp;</span><span style="color:#464646"><br>
</span><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Toenable the CDT site:</span></p>
<p><span style="color:#454B57">1.</span><span style="color:#454B57">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#454B57">Select the Eclipse menu Window→Preferences.</span></p>
<p><span style="color:#464646">2.</span><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#454B57">Select Install/Update→Available SoftwareSites and select the site,&nbsp;<a href="http://download.eclipse.org/tools/cdt/releases/juno" target="_blank"><span style="color:#3E73A0">http://download.eclipse.org/tools/cdt/releases/juno</span></a>.</span></p>
<p><span style="color:#454B57">(HereI used Helios, it's a little different. Keep it Enabled.</span><span style="color:#464646">&nbsp;</span><span style="color:red">Maybe we need to add the link in the lastline</span><span style="color:#454B57">)</span></p>
<p><span style="color:#454B57">3.</span><span style="color:#454B57">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#454B57">Use the Enable button to enable the site.</span></p>
<p><span style="color:#464646">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="color:#454B57">Install the&nbsp;<strong>GDB&nbsp;Hardware</strong>&nbsp;Debuggingplug-in via the&nbsp;</span><span style="color:#464646"><a href="http://wiki.eclipse.org/FAQ_What_is_the_Update_Manager?" title="http://wiki.eclipse.org/FAQ_What_is_the_Update_Manager?"><span style="color:#3E73A0">Update
 Manager</span></a></span><span style="color:#454B57">:</span></p>
<p><span style="color:#454B57">1.</span><span style="color:#454B57">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#454B57">Select the Eclipse menu Help→Install NewSoftware</span></p>
<p><span style="color:#454B57">2.</span><span style="color:#454B57">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#454B57">Select the&nbsp;<a href="http://download.eclipse.org/tools/cdt/releases/indigo" title="http://download.eclipse.org/tools/cdt/releases/indigo"><span style="color:#3E73A0">http://download.eclipse.org/tools/cdt/releases/indigo</span></a>&nbsp;sitein
 the Work With list(Add this, if there isn't.)</span></p>
<p><span style="color:#454B57">3.</span><span style="color:#454B57">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="color:#454B57">Enable the&nbsp;GDB&nbsp;HardwareDebugging plugin under CDT Optional Features</span></p>
<p><span style="color:#454B57">&nbsp;</span></p>
<p><span style="color:#454B57">&nbsp;</span></p>
<p><span style="color:#454B57">&nbsp;</span></p>
<p><span style="color:#454B57">The</span><span style="color:#454B57">&nbsp;</span><span style="color:#454B57">Blackfin plug-ins</span><span style="color:#454B57">&nbsp;</span><span style="color:#454B57">can be obtained from the</span><span style="color:#454B57">&nbsp;</span><span style="color:#454B57"><a href="http://blackfin.uclinux.org/eclipse/" title="http://blackfin.uclinux.org/eclipse/"><span style="color:#336699">http://blackfin.uclinux.org/eclipse/</span></a></span><span style="color:#454B57">&nbsp;</span><span style="color:#454B57">site
 via the</span><span style="color:#454B57">&nbsp;</span><span style="color:#454B57"><a href="http://wiki.eclipse.org/FAQ_What_is_the_Update_Manager?" title="http://wiki.eclipse.org/FAQ_What_is_the_Update_Manager?"><span style="color:#336699">Update Manager</span></a></span><span style="color:#454B57">:</span></p>
<p><span style="color:#828A9F">&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 1.Select Help→Install New Softwarefrom the Eclipse menu</span><span style="color:#828A9F"><br>
</span><span style="color:#828A9F">&nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;2.Click the Add button</span><span style="color:#828A9F"><br>
</span><span style="color:#828A9F">&nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;3.Add&nbsp;<a href="http://blackfin.uclinux.org/eclipse/" target="_blank"><span style="color:#336699">http://blackfin.uclinux.org/eclipse/</span></a>&nbsp;as anew site&quot;Blackfin GNU&quot;</span></p>
<p><a href="http://docs.blackfin.uclinux.org/lib/exe/detail.php?id=toolchain:eclipse:install&amp;media=toolchain:eclipse:junoeclipseidebfupdatesite.png" title="&quot;toolchain:eclipse:junoeclipseidebfupdatesite.png&quot; "></a></p>
<p><span style="color:#454B57">Thenyou can select the plug-ins to install:</span></p>
<p>&nbsp;&nbsp; &nbsp;&nbsp;</p>
<p><span style="color:#454B57">TheBlackfin Debug plug-in results in extra selectable entries in the “DebugConfiguration” dialog, while the Tool chain plug-in allows for selecting thedesired tool chain out of a list from within the New Project Wizard dialog.</span></p>
<p>&nbsp;</p>
<p><a name="zylin_cdt"></a><a name="t0"></a><strong><span style="color:#336699">Install the Zylin CDT</span></strong></p>
<p><span style="color:#454B57">TheEclipse C/C&#43;&#43; Development Tools (CDT) has excellent&nbsp;GDB&nbsp;support.However, there are a few stumbling blocks when trying to debug embeddedapplications, which&nbsp;<a href="http://opensource.zylin.com/embeddedcdt.html" title="http://opensource.zylin.com/embeddedcdt.html"><span style="color:#336699">ZylinAS</span></a>&nbsp;has
 made some modifications in Eclipse CDT to improvesupport for&nbsp;GDB&nbsp;embedded debugging.</span></p>
<p><span style="color:#454B57">Pathfor update manager “Add Site”:&nbsp;<a href="http://opensource.zylin.com/zylincdt" title="http://opensource.zylin.com/zylincdt"><span style="color:#336699">http://opensource.zylin.com/zylincdt</span></a></span></p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;<a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s6.sinaimg.cn/orignal/55a4cddcgd11a621b1c65" target="_blank"></a></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><a name="subclipse_svn"></a><a name="t1"></a><strong><span style="color:#336699">Install Subclipse (SVN) &nbsp; (I skipped this)</span></strong></p>
<p><span style="color:#454B57">Whilenot released with Eclipse, there is a Subversion plugin called&nbsp;<a href="http://subclipse.tigris.org/" title="http://subclipse.tigris.org/"><span style="color:#336699">Subclipse</span></a>. Visit the Subclipse homepage forsome&nbsp;<a href="http://subclipse.tigris.org/install.html" title="http://subclipse.tigris.org/install.html"><span style="color:#336699">conciseinstall
 directions</span></a>.</span></p>
<p><span style="color:#454B57">Onceyou have Subclipse installed, the interface is the same as usingthe&nbsp;CVS&nbsp;plugin.</span></p>
<p>&nbsp;</p>
<p><a name="target_management_terminal"></a><a name="t2"></a><strong><span style="color:#336699">Target Management Terminal</span></strong></p>
<p><span style="color:#454B57">AnANSI (vt102) compatible Terminal including plug-ins for&nbsp;<strong>Serial</strong>,&nbsp;<strong>SSH</strong>&nbsp;and&nbsp;<strong>Telnet</strong>&nbsp;connections.</span></p>
<p><a href="http://docs.blackfin.uclinux.org/lib/exe/detail.php?id=toolchain:eclipse:install&amp;media=toolchain:eclipse:eclipse-target-terminal.jpg" title="&quot;toolchain:eclipse:eclipse-target-terminal.jpg&quot; "></a></p>
<p>To use a serial terminal in Eclipse Juno. (Here we use Helios)</p>
<p><span style="color:#454B57">&nbsp;</span></p>
<p>1: Install the software for serial terminals:</p>
<p>Navigate to:&nbsp;Help&nbsp;-&gt;&nbsp;Install New Software...</p>
<p>Dropdown list for&nbsp;Work with:&nbsp;tosay&nbsp;Juno - http://download.eclipse.org/releases/juno</p>
<p>Select:&nbsp;Mobile and Device Development,especially&nbsp;Target Management Terminal&nbsp;which is &quot;An ANSI(vt102) compatible Terminal including plug-ins for Serial, SSH and Telnetconnections.&quot;</p>
<p>Click&nbsp;Next&nbsp;and anything else to finishthe install ...</p>
<p>2: Open the view</p>
<p>Navigate to:&nbsp;Window&nbsp;-&gt;&nbsp;Show View&nbsp;-&gt;&nbsp;Other ...&nbsp;-&gt;&nbsp;Terminal&nbsp;-&gt;&nbsp;Terminal&nbsp;(NOTE:singular&nbsp;Terminal, not plural&nbsp;Terminals)</p>
<p>3: Open a terminal</p>
<p>The rest should be fairly obvious as the view contains icons toConnect, Disconnect, Settings, etc which are related to Serial, SSH and Telnetconnections.</p>
<p>Reference: click&nbsp;<a href="http://stackoverflow.com/questions/12140381/how-to-open-a-serial-terminal-in-eclipse-juno" target="_blank"><span style="color:#336699">here</span></a></p>
<p><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s16.sinaimg.cn/orignal/55a4cddcg7b4f77880e6f" target="_blank"></a></p>
<p>&nbsp;</p>
<p><span style="color:#454B57">TargetManagement Terminal Serial Connector requires&nbsp;<a href="http://rxtx.qbang.org/wiki/index.php/Main_Page" title="http://rxtx.qbang.org/wiki/index.php/Main_Page"><span style="color:#336699">RXTX</span></a></span></p>
<p><span style="color:#454B57">Installationas an Eclipse Plugin via Update Manager:</span></p>
<div align="left">
<hr size="0" width="100%" noshade="noshade" align="left" style="color:#454B57">
</div>
<p><span style="color:#454B57">InEclipse, choose Help &gt; Software Updates…</span></p>
<p><span style="color:#454B57">AddNew Remote Site:</span></p>
<p>Name = RXTX URL =http://rxtx.qbang.org/eclipse/</p>
<p><span style="color:#454B57">&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;</span>&nbsp;<span style="color:#B9D478">&nbsp;</span><strong><span style="color:#6C0000">·&nbsp;</span></strong><span style="color:#454B57">select proper version, InstallAll</span></p>
<p><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s8.sinaimg.cn/orignal/55a4cddcgd11ad0a75657" target="_blank"></a></p>
<p>&nbsp;</p>
<p><span style="color:#111111">Untill Now, we have i</span>nstalled the components:&nbsp;&nbsp;</p>
<p><span style="color:#111111">BlackfinucLinux</span><span style="color:#111111">&nbsp;</span><span style="color:#111111">(</span><span style="color:red">Blackfin plug-ins</span><span style="color:#111111">)</span></p>
<p><span style="color:#111111">GDB Hardware Debugging</span></p>
<p><span style="color:#111111">Target Terminal Management</span></p>
<p><span style="color:#111111">ZynxCDT (</span><span style="color:red">We installed ZylinCDT instead</span><span style="color:#111111">)</span></p>
<p>&nbsp;</p>
<p><a name="t3"></a><strong><span style="color:#111111">Section3.Library Installation</span></strong></p>
<p>&nbsp;</p>
<p><span style="color:#111111">Checkout of common code</span></p>
<div style="background:#EEECE1">
<p>&gt;svn co&nbsp;<a href="https://nuforge.coe.neu.edu/svn/pal/tags/v1.1_gnu/0.1/bare-c"><span style="color:#336699">https://nuforge.coe.neu.edu/svn/pal/tags/v1.1_gnu/0.1/bare-c</span></a></p>
</div>
<p><span style="color:#111111">Edit Makefile.macros located in bare-c/common</span></p>
<p><span style="color:#111111">Edit the prefix to the path where you want the library to beinstalled&nbsp; For ex:&nbsp;</span></p>
<p>&gt; # --- Installationdirectories prefix =<span style="color:#111111"> /home/student/agarwal/temp/pal</span></p>
<p><span style="color:#111111">Edit the COMPILER macros to the path where you have installedthe compiler above</span><span style="color:#111111"><br>
</span><span style="color:#454B57">The defaultinstall path for all of our toolchains is&nbsp;</span><span style="color:#454B57">/opt/uClinux/</span></p>
<div style="background:#EEECE1">
<p>#-- Compiler COMPILER = &nbsp;&lt;COMPILER_NSTALLATION_DIR&gt;/bfin-elf/bin/bfin-elf-gcc</p>
</div>
<p>&nbsp;<br>
<span style="color:#111111">eg. </span>#-- Compiler COMPILER= /opt/uClinux/bfin-elf/bin/bfin-elf-gcc</p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">BackUP:</span><span style="color:#111111"><br>
</span>#-- Compiler<br>
COMPILER = /ECEnet/Apps1/sce-ext-pkg/sw/bfin_toolchain.2011R1-BETA1/bfin-elf</p>
<p><span style="color:#111111">For Compiling the library we need to source the blackfincompiler path .</span></p>
<p><span style="color:#111111">For tcsh shell&nbsp;</span><span style="color:#111111"><br>
</span><span style="color:#111111">&gt;&nbsp;setenv PATH &lt;COMPILER_NSTALLATION_DIR&gt;/bfin-elf/bin/:$PATH&nbsp;</span><span style="color:#111111"><br>
</span><span style="color:#111111">For bash shell&nbsp;</span><span style="color:#111111"><br>
</span><span style="color:#111111">&gt; export PATH;PATH=&lt;COMPILER_NSTALLATION_DIR&gt;/bfin-elf/bin:$PATH</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; Here we use:</span></p>
<div style="background:#EEECE1">
<p>&gt;export PATH; PATH = /opt/uClinux/bfin-elf/bin:$PATH&nbsp;</p>
</div>
<p><span style="color:#111111">Follow these steps to install the libraries</span></p>
<div style="background:#EEECE1">
<p>&gt;cd common &nbsp;<br>
&gt; make clean&nbsp;<br>
&gt; make&nbsp;<br>
&gt; sudo make install</p>
</div>
<p><span style="color:#111111">&nbsp; Validate your environment&nbsp;&nbsp;&nbsp;</span></p>
<div style="background:#EEECE1">
<p>&gt;source &lt;installation path&gt;/bin/setup.sh&nbsp;</p>
</div>
<p>&nbsp;eg.&gt; source /usr/uClinux/bfin-elf/bin/setup.sh</p>
<div style="background:#EEECE1">
<p>&gt;cd demo&nbsp;<br>
&gt; make clean&nbsp;<br>
&gt; make</p>
</div>
<p><span style="color:#111111">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; This make shouldcomplete without error. (</span><span style="color:red">But we meet several errors</span><span style="color:#111111">)</span></p>
<p><span style="color:#111111">&nbsp; ==============================================</span></p>
<p><span style="color:#111111">Here, I just referred some other instructions, and:</span></p>
<p><span style="color:#111111">&nbsp;1.deleted Eclipse,&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;2.Downloaded the Eclipse(Juno)</span></p>
<p><span style="color:#111111">&nbsp;3.Set the PATH</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">1. ReInstall the Eclipse</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; go to the &quot;eclipse&quot; folder:</span></p>
<div style="background:#EEECE1">
<p>&gt;cd /opt</p>
<p>&gt;rm -rf eclipse</p>
</div>
<p><span style="color:#111111">2. Download the Eclipse (Juno)</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp;&nbsp;</span><span style="color:#111111">Select the Juno version,&nbsp;Download the eclipsepackage from:&nbsp;</span><span style="color:#111111"><a href="http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/juno/SR1/eclipse-cpp-juno-SR1-linux-gtk.tar.gz" target="_blank"><span style="color:#336699">Linux
 32bit</span></a></span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;Extra it into/home/yourname/programfiles/</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;We have Installed the Java RuntimeEnvironment above, so we don't need to install it here.</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">3. Configure the</span>&nbsp;<span style="color:#111111">environment variable(PATH)</span></p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;&nbsp;Modify the environment variable of &quot;bfin-linux-uclibc-gcc&quot;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; In Terminal, run:</span></p>
<div style="background:#EEECE1">
<p>&gt;sudo gedit /etc/profile</p>
</div>
<p><span style="color:#111111">&nbsp;&nbsp; &nbsp; &nbsp;&nbsp;Modify the EnvironmentalVariable. Note, this is user environmental variable, After this we can only use</span></p>
<p><span style="color:#111111">&quot;bfin-linux-uclibc-gcc&quot; but cannot use&quot;sudo&nbsp;bfin-linux-uclibc-gcc&quot;. All the command after&quot;sudo&quot; is for using&nbsp;</span></p>
<p><span style="color:#111111">system environmental variable but not user's environmentalvariable.</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;In the gedit window add the linebelow at the tail.</span></p>
<div style="background:#EEECE1">
<p>&nbsp;&gt;export PATH=$PATH:/opt/uClinux/bfin-linux-uclibc/bin&nbsp;</p>
</div>
<p><span style="color:#111111">&nbsp; &nbsp;&nbsp;</span><span style="color:#111111">And for the JRE PATH configure, add the line below:</span></p>
<p>&nbsp;&gt;JAVA_HOME=/opt/java/32/jre1.6.0_16<br>
&nbsp;&gt; PATH=$JAVA_HOME/bin:$PATH</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp;See the snap below:</p>
<p><span style="color:#111111">&nbsp; &nbsp;&nbsp;</span><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s8.sinaimg.cn/orignal/55a4cddcgd12acb47ed37" target="_blank"></a></p>
<p>&nbsp;</p>
<p><span style="color:#111111">4. Building a uClinux Hello World project&nbsp;</span><span style="color:#111111"><br>
</span><span style="color:#111111">&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;Go to the location ofeclipse:&quot;/home/yourname/programfiles/eclipse&quot;, Run the eclipse by:</span></p>
<div style="background:#EEECE1">
<p>&gt;cd&nbsp;/home/yourname/programfiles/eclipse<br>
&gt; ./eclipse&nbsp;</p>
</div>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; In Eclipse,New-&gt;Project-&gt;C/C&#43;&#43; -&gt; C Project -&gt; &quot;Project type:Executable/Empty Project&quot; and&nbsp;</span></p>
<p><span style="color:#111111">&quot;ToolChains:Cross GCC &quot; -&gt; &quot;Next&quot; -&gt;Set the path of your &quot;bfin-linux-uclibc-gcc&quot; &nbsp;-&gt; finish.</span></p>
<p><span style="color:#111111">See the snap:</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;</span><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s16.sinaimg.cn/orignal/55a4cddcgd12af44ee9ef" target="_blank"></a></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="color:#111111">And build your Hello Worldproject:</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;</span><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s10.sinaimg.cn/orignal/55a4cddcgd12b02c1a3a9" target="_blank"></a></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; This method is only for building theuClinux app. If you want to build Bare-Metal&nbsp;</span></p>
<p><span style="color:#111111">Refer to:</span><span style="color:#111111"><br>
</span><span style="color:#111111">DevelopingBare-Metal C Applications</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; Setup common bare-c library&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=bareC"><span style="color:#336699">bareC</span></a>&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=bareCReleaseNotes"><span style="color:#336699">Release
 Notes</span></a></span></p>
<p><span style="color:#111111">&nbsp; &nbsp; Update bare-c libraries&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=updateBarec"><span style="color:#336699">updateBarec</span></a></span></p>
<p><span style="color:#111111">&nbsp; &nbsp; Doxygen documentation&nbsp;<a href="http://www.ece.neu.edu/~esl/EECE4534/common/"><span style="color:#336699">link</span></a></span></p>
<p><span style="color:#111111">&nbsp; &nbsp; Developing and debugging C applications usingEclipse&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=eclipseDev"><span style="color:#336699">link</span></a></span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;TLL's: Bare-C Development/Debugging withEclipse<a href="https://tll.assembla.com/spaces/designcenter/wiki/Developing_C_Applications_in_Baremetal"><span style="color:#336699">&nbsp;link</span></a></span><span style="color:#111111"><br>
</span><span style="color:#111111">&nbsp; &nbsp; &nbsp;More detailed debug tutorial.</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;Debug connection through gnICE&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?&amp;pageid=316#gdbProxy"><span style="color:#336699">link</span></a></span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;Alternative debug connection: usingkgdb&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=kgdb"><span style="color:#336699">kgdb</span></a></span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;Using DSP-specific libraries&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=dsplibs"><span style="color:#336699">dsplibs</span></a></span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;Launching bare-c apps through U-Boot&nbsp;<a href="https://tll.assembla.com/spaces/designcenter/wiki/Transferring_Files_and_Launching_Bare_C_Apps"><span style="color:#336699">link</span></a></span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;Analog Devices introduction to bare-cdevelopment&nbsp;<a href="http://docs.blackfin.uclinux.org/doku.php?id=toolchain:bare_metal"><span style="color:#336699">link</span></a></span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;Stop automatically booting linux&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=u-boot"><span style="color:#336699">u-boot</span></a></span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">================= Connect with Target Board =================</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; In this Section, we are going todownload the executable bin file made by Eclipse down to target.</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; 1. Install the minicom</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; 2. Install the USB-UART bridgeconverter driver on Linux</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; 3. Install the USB-Ethernet bridgeconverter driver on Linux, and Configure it.</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; 4. Test the Communication functions.</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; 5. Test the J-TAG connection if Ihave interests.</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;&nbsp;&nbsp;</span><span style="color:#111111">1. Install the minicom</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;In the Terminal, input:</span></p>
<div style="background:#EEECE1">
<p>&gt;sudo apt-get install minicom</p>
</div>
<p>&nbsp; &nbsp;&nbsp;<span style="color:#111111">2. Install the USB-UART bridge converter driver on Linux</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;Install driver for USB-UART bridge converter on Linux Ubuntu12.04</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;&nbsp;&nbsp;</span><span style="color:#111111">Ubuntu</span><span style="color:#111111">下USB</span>转串口芯片驱动程序安装，支持cp210x,pl2303等</p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;&nbsp;&nbsp;Reference:&nbsp;<a href="http://pharos.ece.utexas.edu/wiki/index.php/Fixing_the_cp210x_open_-_Unable_to_enable_UART_Error_-_04/17/2011" target="_blank"><span style="color:#336699">Fixing the cp210x open - Unable toenable UART
 Error</span></a></span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;&nbsp;&nbsp;When you plugin your USB-UART converter, and run &quot;</span><strong><span style="color:#111111">&gt; ls /dev/tty*</span></strong><span style="color:#111111">&quot;, if you don'tsee the<br>
&nbsp;/dev/ttyUSB0 (or similar), your Linux does not detect your USB-UARTdevice.</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">We need to install the driverfor your device.</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">Here we use Ubuntu12.04, andUpdated the source to 3.2.0 version. If there is difference about&nbsp;<br>
version Number from your OS platform, please try to modify it into yours.</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">1.Download the Linux Source Code</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">Open a terminal and executethe following commands. Note that your version of Linux may differ&nbsp;<br>
slightly -- adjust accordingly.</span></p>
<div style="background:#EEECE1">
<p>$cd ~</p>
<p>$sudo apt-get install build-essential linux-source</p>
<p>$cp /usr/src/linux-source-3.2.0.tar.bz2 .</p>
<p>$bunzip2 linux-source-3.2.0.tar.bz2&nbsp;</p>
<p>$tar xf linux-source-3.2.0.tar&nbsp;</p>
<p>$cd ~/linux-source-3.2.0</p>
</div>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">2.Recompile and Reinstall the cp210x Driver</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">From within a terminal,execute:</span></p>
<div style="background:#EEECE1">
<p>$cd ~/linux-source-3.2.0</p>
<p>$make oldconfig</p>
<p>$make prepare</p>
<p>$make scripts</p>
<p>$cp /usr/src/linux-headers-3.2.0-34-generic-pae/Module.symvers .</p>
</div>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">Here, I have the&quot;3.2.0-29&quot; version also, I launched the command above, but not thebelow:</span></p>
<div style="background:#EEECE1">
<p>&nbsp;&quot;cp /usr/src/linux-headers-3.2.0-29-generic-pae/Module.symvers .&quot;</p>
</div>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">Recompile and Reinstall thecp210x Driver</span></p>
<p><span style="color:#111111">Here, We can actually installmany kinds of USB-UART converter drivers. We take cp210x as the<br>
&nbsp;example.</span></p>
<p><span style="color:#111111">From within a terminal,execute:</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<div style="background:#EEECE1">
<p>$make M=drivers/usb/serial</p>
<p>$sudo mv /lib/modules/$(uname -r)/kernel/drivers/usb/serial/cp210x.ko/lib/modules<br>
&nbsp; /$(uname -r)/kernel/drivers/usb/serial/cp210x.ko.old</p>
<p>$sudo cp drivers/usb/serial/cp210x.ko /lib/modules/$(uname-r)/kernel/drivers/usb<br>
&nbsp; /serial/</p>
<p>$sudo modprobe -r cp210x</p>
<p>$sudo modprobe cp210x</p>
</div>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">Reboot Linux system.</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">Run Terminal:&nbsp;</span></p>
<div style="background:#EEECE1">
<p>$ls /dev/tty*</p>
</div>
<p><span style="color:#111111">The we can see the device isdetected by Linux Host OS:</span></p>
<p><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s1.sinaimg.cn/orignal/55a4cddcgd12f287c93b0" target="_blank"></a></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;Reference:&nbsp;<a href="http://pharos.ece.utexas.edu/wiki/index.php/Fixing_the_cp210x_open_-_Unable_to_enable_UART_Error_-_04/17/2011" target="_blank"><span style="color:#336699">http://pharos.ece.utexas.edu/wiki/index.php/Fixing_the_cp210x_open_-_Unable_to_enable_UART_Error_-_04/17/2011</span></a></span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; Then, We can configure the minicom to communicatewith our target board.</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">======================================================</span></p>
<p><span style="color:#111111">Here is an example ofconfigure the parameters of minicom for TLL6527M PAL board:</span></p>
<p><span style="color:#111111">I just copied it here hardlywithout any font editing. Sorry about that.</span></p>
<p><a name="t4"></a><strong><span style="color:#111111">Serial Communications from Host-PC to TLL6527M Target Hardware</span></strong></p>
<p><span style="color:#111111">TheTLL System Design Environment (SDE) running on the host PC comes pre-configuredwith<br>
&nbsp;the required settings for serial-UART communications between the host PCand the TLL6527M<br>
&nbsp;base module. Generally, for use with TLL6527M the settings need not to bechanged&nbsp;<br>
(Note: From the SDE OS version 0.3.2, the serial terminal program Minicom isset to&nbsp;<br>
open /dev/ttyUSB0 with the needed configuration as mentioned above.).</span></p>
<p><span style="color:#111111">Tostart serial communication from host to the TLL6527M, open the serialcommunication terminal&nbsp;<br>
program Minicom. Just open a terminal window on your Linux SDE running on thehost PC&nbsp; and&nbsp;<br>
type the command &quot;minicom&quot; at the command prompt on your host PCterminal console.</span></p>
<p><span style="color:#111111">TheTLL6527M on power-up&nbsp; is set by default to the following serial-UARTcommunication settings:</span></p>
<p><span style="color:#111111">Bitsper second = 115200</span></p>
<p><span style="color:#111111">Databits = 8</span></p>
<p><span style="color:#111111">Parity= None</span></p>
<p><span style="color:#111111">StopBits = 1</span></p>
<p><span style="color:#111111">Flowcontrol = None</span></p>
<p><span style="color:#111111">&nbsp;Makesure that the TLL6527M's USB-UART port has been mapped to a ttyUSBx device nodein the /dev&nbsp;<br>
folder on the host PC SDE. See following example:</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p></p>
<p><span style="color:#111111">Nowmake sure that this &quot;ttyUSBx&quot; is set as the destination port, baudrate is 115200,<br>
&nbsp;and 8N1 mode for minicom, by running the command 'minicom -s'. Note thatrunning the&nbsp;<br>
&quot;minicom -s&quot; command as a normal user will only apply the settingsfor the current&nbsp;<br>
session. In order to make the settings permanent, these commands need to be runwith&nbsp;<br>
root privileges.See example below:&nbsp;</span></p>
<p></p>
<p><span style="color:#111111">Thefollowing window will appear on your host PC terminal window:&nbsp;</span></p>
<p></p>
<p><span style="color:#111111">Goto ‘Serial port setup’, following screen will appear, make sure that serialdevice is set to ‘/dev/ttyUSB0’&nbsp;<br>
and bps/par/bits is set to 115200 8N1:</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">Nowgo back to the main menu and choose ‘save setup as dfl’. This will save theconfiguration settings and from next time&nbsp;<br>
onwards user can directly run ‘minicom’ command without the option ‘–s’ and itwill load settings from the saved file.</span></p>
<p></p>
<p><span style="color:#111111">RESETthe target by using the SDE's TLL6527M reset utility or by pressing the RESETbutton on the target.&nbsp;</span></p>
<p><span style="color:#111111">Followingmessages will be displayed on the terminal. By default, U-Boot is setupto&nbsp;<br>
automatically start booting the OS after waiting for a few seconds for the userto&nbsp;<br>
interrupt the automatic launch by pressing any key. So to stop U-Boot fromloading&nbsp;<br>
uClinux and provide U-Boot command prompt, just hit any key. (This is assumingTLL6527M&nbsp;<br>
is flashed with u-boot and uClinux. TLL6527Ms are pre-flashed with firmwarebefore shipping)</span><span style="color:#111111">.</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Go to the website of yourconverter applier, for me:&nbsp;<span style="color:#111111"><a href="http://www.silabs.com/products/mcu/Pages/USBtoUARTBridgeVCPDrivers.aspx" target="_blank"><span style="color:#336699">Here</span></a></span>, Download&nbsp;</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;I halted here. :( But I nevergive up, solved it next:)</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; Here is another simpler way to install thedriver:&nbsp;<br>
&nbsp;<a href="http://blog.sina.com.cn/s/blog_55a4cddc0101afs8.html" target="_blank"><span style="color:#336699">http://blog.sina.com.cn/s/blog_55a4cddc0101afs8.html</span></a></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p>&nbsp; &nbsp;&nbsp;3. Minicom configuration:</p>
<p>&nbsp;&nbsp; &nbsp;See:&nbsp;<a href="http://blog.sina.com.cn/s/blog_55a4cddc0101afs8.html" target="_blank"><span style="color:#336699">http://blog.sina.com.cn/s/blog_55a4cddc0101afs8.html</span></a></p>
<p>&nbsp;&nbsp;To here, we canuse Eclipse to develop our own Blackfin-uClinux app and send it down to ourtarget</p>
<p>&nbsp;&nbsp; &nbsp; board (TLL6527M PAL Board), But there is still no our desiredToolChain&nbsp;<span style="color:#454B57">outof a list from within the New Project Wizard dialog</span></p>
<p>&nbsp;&nbsp; &nbsp;&nbsp;</p>
<p>&nbsp;</p>
<p><span style="color:#111111">==========</span><span style="color:#111111">&nbsp;</span><span style="color:#111111">Add the</span><span style="color:#111111">&nbsp;</span><span style="color:#111111">Blackfin Linux FDPIC GNU Toolchain(bfin-linux-uclibc)&nbsp;==========</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>Then I installed the Blackfin Plug-ins againfor Juno</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;Reference :&nbsp;</span><a href="http://docs.blackfin.uclinux.org/doku.php?id=toolchain:eclipse:install" target="_blank"><span style="color:#336699">Installing Eclipse 4.2 (Juno)</span></a></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>There contains:</p>
<p><span style="color:#111111">&nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>`. A Java Runtime Environment (JRE) version1.6 or newer</p>
<p><span style="color:#111111">&nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>`. Eclipse IDDE (version 4.2 recommended)</p>
<p><span style="color:#111111">&nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>`. C/C&#43;&#43; Development Tools (CDT)</p>
<p><span style="color:#111111">&nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>`. Blackfin toolchain</p>
<p><span style="color:#111111">&nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>`. Blackfin Plug-ins</p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;Here we only installed thePlug-ins:</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;The Blackfin plug-ins willrequire Eclipse 4.2, as well as the CDT.&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;Additionally, you will have toinstall the GDB Hardware Debugging&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;CDT plug-in (it is usually notinstalled by default). To install&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;the GDB Hardware Debuggingplug-in the CDT update site needs enabled.</span></p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;--------</span></p>
<p><span style="color:#111111">&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;To enable the CDT site:</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>1. Select the Eclipse menuWindow→Preferences.</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>2. Select Install/Update→Available SoftwareSites and select the site,&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>3.http://download.eclipse.org/tools/cdt/releases/juno.</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>4. Use the Enable button to enable the site.</p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>---------</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>Install the GDB Hardware Debugging plug-invia the Update Manager:</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>1. Select the Eclipse menu Help→Install NewSoftware</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>2. Select thehttp://download.eclipse.org/tools/cdt/releases/indigo site in the Work Withlist</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>3. Enable the GDB Hardware Debugging pluginunder CDT Optional Features</p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>---------</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>The Blackfin plug-ins can be obtained fromthe&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span><u><span style="color:#874F85">http://blackfin.uclinux.org/eclipse/</span></u>&nbsp;&nbsp; &nbsp;site via theUpdate Manager:</p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>1. Select Help→Install New Software from theEclipse menu</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>2. Click the Add button</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>3. Add http://blackfin.uclinux.org/eclipse/as a new site &quot;Blackfin GNU&quot;</p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>---------</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>Then you can select the plug-ins to install:</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>The&nbsp;<strong>Blackfin Debug plug-in</strong>&nbsp;resultsin extra selectable entries in the&nbsp;“Debug Configuration” dialog,&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;T</span>he&nbsp;<strong>Tool chain plug-in</strong>&nbsp;allows&nbsp;for selecting the desired tool chain out of alist from within the&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>New Project Wizard dialog.</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>As the dialoge box said, restart Eclipse.</p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>---------</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>Then when we creat a C Project, &quot;Projecttype&quot;:Empty Project, in the &quot;ToolChains&quot; box,&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>there appears:&quot;Blackfin Linux FDPIC GNUToolchain(bfin-linux-uclibc)&quot;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;See the Snap pic below:</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s5.sinaimg.cn/orignal/55a4cddcgd1396bb87584" target="_blank"></a></p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">======================================</span></p>
<p><span style="color:#111111">=========== Ethernet Connection ==========</span></p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; </span><span style="color:#111111">Here, I'm going to configure the Ethernet manually.</span></p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; I tried to run palTelnet.sh, butfailed by some errors.:</span></p>
<p><span style="color:#111111">-----------</span></p>
<p><span style="color:#111111">./palTelnet.sh: 44: read: arg count</span></p>
<p><span style="color:#111111">WARNING: TRENDnet TU2-ET100 not found.</span></p>
<p><span style="color:#111111">&nbsp; 1) Validate USB connection from TU2-ET100 to host.</span></p>
<p><span style="color:#111111">&nbsp; 2) Validate TU2-ET100 is connected to virtual machineinstead of host).</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;check: Virtual Machine -&gt; RemovableDevices -&gt; asix ax88772</span></p>
<p><span style="color:#111111">Press &lt;ENTER&gt; to continue, &lt;CTRL&gt;-C to stop</span></p>
<p><span style="color:#111111">./palTelnet.sh: 44: read: arg count</span></p>
<p><span style="color:#111111">-----------</span></p>
<p align="left"><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; Itsays the TRENDnet TU2-ET100, I thought it was because of the driver ofTU2-ET100 was not configured.</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; (The gray part does not work, youcan just skip it)</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;</span>&nbsp;<span style="color:#A4A4B2">Download the driver from:</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp;http://www.trendnet.com/downloads/list_subcategory.asp?SUBTYPE_ID=1003</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; Extract it into:</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp;/home/sonictl/Downloads/LINUX2.6.9_REV122/AX88772_772A_LINUX2.6.9_REV122/</span></p>
<p><span style="color:#A4A4B2">open readme, it says:&quot;This driver is only for Kernel 2.6.9to 2.6.13&quot;</span></p>
<p><span style="color:#A4A4B2">It seems like I need to download the Kernel2.6.9. get it&nbsp;</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; from:get&quot;linux-2.6.9.tar.bz2&quot; from: www.kernel.org/pub/linux/kernel/v2.6/</span></p>
<p>&nbsp;</p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; Open a terminal and execute thefollowing commands.</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cd ~</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ sudo mv~/Downloads/linux-2.6.9.tar.bz2 /usr/src/</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cp/usr/src/linux-2.6.9.tar.bz2 .</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ bunzip2 linux-2.6.9.tar.bz2</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ tar xf linux-2.6.9.tar&nbsp;</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cd ~/linux-2.6.9</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; Recompile and Reinstall the cp210xDriver,From within a terminal, execute:</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cd ~/linux-2.6.9</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ make oldconfig</span></p>
<p><span style="color:#A4A4B2">I pressed all Return Key.</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ make prepare</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ make scripts</span></p>
<p><span style="color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cp/usr/src/linux-headers-3.2.0-34-generic-pae/Module.symvers .</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></p>
<p><span style="color:#111111">To here I get halted. I searched online for a while, and Did thefollowing:</span></p>
<p><span style="color:#111111">========================================================</span></p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span><span style="color:#111111">Retry Install the ASIX AX88772Driver for Linux</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span><span style="color:#111111">Download the driver for Kernel3.x</span><span style="color:#111111">&nbsp;</span><span style="color:#111111">（</span><span style="color:#111111">my kernel is&nbsp;3.2.0, you can update your kernel
 by&nbsp;</span></p>
<div style="background:#EEECE1">
<p>&gt;&nbsp;sudoapt-get install build-essential linux-source&nbsp;</p>
</div>
<p><span style="color:#111111">&nbsp;unzip the Package_YouDownloaded.zip</span></p>
<p><span style="color:#111111">&nbsp;go into the folder, and open the readme file in gedit</span></p>
<p><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s15.sinaimg.cn/orignal/55a4cddcgd140587f2ede" target="_blank"></a></p>
<div style="background:#EEECE1">
<p>$sudo su</p>
<p>#&nbsp;<span style="background:#FFFCF6">make</span></p>
</div>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;If the compilation is well, the</span><span style="color:#111111">&nbsp;</span><strong><span style="color:#111111">asix.ko</span></strong><span style="color:#111111">&nbsp;</span><span style="color:#111111">will be created under
 the current</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; directory.</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;If you want to use modprobecommand to mount the driver, executing the</span></p>
<p><span style="color:#111111">&nbsp; &nbsp;&nbsp;</span><span style="color:#111111">following command to install the driver into your Linux:</span></p>
<div style="background:#EEECE1">
<p>#&nbsp;[root@localhosttemplate]# make install</p>
</div>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;----------------</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;</span><span style="color:#111111">Usage:</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;----------------</span></p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;1.&nbsp;</span><span style="color:#111111">If you want to load the drivermanually, go to the driver directory and</span></p>
<p><span style="color:#111111">&nbsp; &nbsp;&nbsp; &nbsp; &nbsp;</span><span style="color:#111111">execute the following commands:(</span>&nbsp;<span style="color:red">this does not work, skip</span>&nbsp;<span style="color:#111111">)</span></p>
<p>&nbsp;</p>
<div style="background:#EEECE1">
<p>[root@localhosttemplate]# insmod asix.ko</p>
</div>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;2.&nbsp;</span><span style="color:#111111">If you had installed the driverduring driver compilation, then you can use</span></p>
<p><span style="color:#111111">&nbsp; &nbsp;&nbsp; &nbsp; &nbsp;</span><span style="color:#111111">the following command to loadthe driver automatically. (</span><span style="color:red">I used this one</span><span style="color:#111111">)</span></p>
<p>&nbsp;</p>
<div style="background:#EEECE1">
<p>[root@localhostanywhere]# modprobe asix</p>
</div>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp;</span><span style="color:#111111">If</span><span style="color:#111111">you want to unload the driver, just executing the followingcommand:</span></p>
<p>&nbsp;</p>
<div style="background:#EEECE1">
<p>[root@localhostanywhere]# rmmod asix</p>
</div>
<p><span style="color:#4644FF">&nbsp; &nbsp; &nbsp;</span>----------------</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;Test if the driver is loaded:</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;But How?</p>
<p>&nbsp;</p>
<p>==================================</p>
<p>&nbsp;&nbsp;&nbsp; &nbsp; &nbsp; So, Ibegan to configure the Host Manually.</p>
<p>&nbsp;</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; First, check the USB devices connected on Ubuntu Linux:</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&gt;lsusb</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; we get:</p>
<p>-----------</p>
<div style="background:#EEECE1">
<p>&nbsp;&nbsp; &nbsp; &nbsp; $ lsusb</p>
</div>
<p>&nbsp;&nbsp; &nbsp; &nbsp; Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 roothub</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; Bus 001 Device 002: ID 80ee:0021 VirtualBox USB Tablet</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; Bus 001 Device 006: ID 10c4:ea60 Cygnal IntegratedProducts, Inc. CP210x Composite Device</p>
<p>&nbsp;&nbsp; &nbsp; &nbsp; Bus 001 Device 007: ID 0b95:7720 ASIX Electronics Corp.AX88772</p>
<p>&nbsp;</p>
<p><a name="t5"></a><strong><span style="color:#111111">Manually Setting IP Address</span></strong></p>
<p>Thissection shows what happens inside the script described in section 2 and allowyou to follow the</p>
<p>setup&nbsp;manually.</p>
<p><span style="color:#111111">1. Identify the Ethernet Interface Name inside the VirtualMachine</span></p>
<p>Each timethe Ethernet adapter is connected a different name may be associated with it.To list</p>
<p>the currentlyknown Ethernet interfaces, open a new terminal, and execute</p>
<div style="background:#EEECE1">
<p>&gt;ifconfig</p>
</div>
<p><span style="color:#111111">Ifconfig will list two connections.</span><span style="color:#111111">&nbsp;<br>
</span><span style="color:#111111">&nbsp;&nbsp; &nbsp;A) Interface with IP address (this is the connection to theinternet), Example output:</span></p>
<div style="background:#EEECE1">
<p>eth0Link encap:Ethernet HWaddr 00:0C:29:51:9B:E6&nbsp;<br>
<strong><span style="color:red">inet addr:</span></strong>192.168.64.138&nbsp;Bcast:192.168.64.255 Mask:255.255.255.0&nbsp;<br>
<u>inet6 addr: fe80::20c:29ff:fe51:9be6/64 Scope:Link</u>&nbsp;<br>
UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1</p>
</div>
<p><span style="color:#111111">B) Interface without IP address (connection to PAL)</span></p>
<p><span style="color:#111111">The device #: 00:50:B6:xx:xx:xx is your Ethernet Adapter #</span></p>
<div style="background:#EEECE1">
<p>eth1Link encap:Ethernet&nbsp;<span style="color:red">HWaddr 00:50:B6</span>:0A:2A:1A&nbsp;<br>
<u>inet6 addr: fe80::20c:29ff:fe51:9be7/64Scope:Link&nbsp;</u><br>
UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1</p>
</div>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;The PAL interface can berecognized by&nbsp;</span></p>
<p>&nbsp;</p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;(1) a different second line: the line startingwith &quot;inet addr&quot; is missing&nbsp;(even though the &quot;inet6addr&quot; may be listed)</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;(2)the HWaddr starts with 00:50:B6.</span></p>
<p><span style="color:#111111">2. Set IP address for interface connected to PAL</span></p>
<p><span style="color:#111111">Assign IP address by using the following command&nbsp;</span><span style="color:#111111"><br>
</span><span style="color:#111111">(weassume&nbsp;<strong>eth1</strong>&nbsp;is the Ethernet connection without specified IPaddress)</span></p>
<div style="background:#EEECE1">
<p>&gt;sudo ifconfig&nbsp;<strong><u>eth1</u></strong>&nbsp;192.168.40.1</p>
</div>
<p><span style="color:#111111">&quot;sudo&quot; allows executing a command as root(administrator). &nbsp;For VM pal user, PW:real happy&nbsp;</span></p>
<p><span style="color:#111111">3. Validate that the interface has accepted the IP setting</span></p>
<p><span style="color:#111111">Use &quot;ifconfig&quot; again</span></p>
<p><span style="color:#111111">4. Establish a Telnet connection to the board.</span></p>
<p><span style="color:#111111">Each PAL is set by default to the IP 192.168.40.211 (you can use&quot;</span><span style="color:#111111">ifconfig</span><span style="color:#111111">&quot; through minicom to get it)</span></p>
<p><span style="color:#111111">Open a telnet session by</span></p>
<div style="background:#EEECE1">
<p>&gt;telnet 192.168.40.211</p>
</div>
<p><span style="color:#111111">For my target PAL board, Username: root, PW:uClinux</span></p>
<p><span style="color:#111111">I Set it succesfully, So the Ethernet connection is configuredcorrectly. That means our driver installation is&nbsp;Done!</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">========== &nbsp;Configure your telnet connection with thetarget board &nbsp; =======</span></p>
<p><span style="color:#111111">Sometimes, when our automatically network connection is notstable, i.e. the DHCP can not work proplerly,</span><span style="color:#111111"><br>
</span><span style="color:#111111">&nbsp;we need the mannually conifigure the ip for our telnet.</span></p>
<p><span style="color:#111111">Manually Configuring Ethernet</span></p>
<p><span style="color:#111111">This section describes how to manually configure the Ethernet connectionon SDE 5.3.x. This is an&nbsp;<br>
alternative to&nbsp;running the automated setup script add-trendnet-adapter.py.</span></p>
<p><a name="t6"></a><strong><span style="color:#111111">1. Connect TU2-ET100and make sure it is connected to the Virtual Machine (see instructions&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=UcLinux&#43;Connection"><span style="color:#336699">here</span></a>)</span></strong></p>
<p><a name="t7"></a><strong><span style="color:#111111">2. Open context menu toto configure connections through NetworkManager.</span></strong></p>
<p><span style="color:#111111">Click right mouse button on NetworkManager icon</span></p>
<p><span style="color:#111111">Select &quot;Edit Connections ...&quot;</span></p>
<p><a name="t8"></a><strong><span style="color:#111111">3. In the dialog box&quot;Network Connections&quot;, select the Wired Connection that correspondsto your USB</span><span style="color:#111111"><br>
</span><span style="color:#111111">Ethernet adapter.</span></strong></p>
<p><span style="color:#111111">Note which connection has appeared after plugging in the USBEthernet adapter. The image below,&nbsp;<br>
assumes&nbsp;&nbsp;&quot;eth0&quot; is the connection for the TU2-ET100.</span></p>
<p><span style="color:#111111">Then, select &quot;Edit ...&quot;</span></p>
<p><a name="t9"></a><strong><span style="color:#111111">4. Validate that theTU2-ET100 was correctly selected</span></strong></p>
<p><span style="color:#111111">The Device MAC address should start with 00:50:b6.</span></p>
<p><a name="t10"></a><strong><span style="color:#111111">5. Select Tab&quot;IPV4 Settings&quot;, and configure the interface.</span></strong></p>
<p><span style="color:#111111">Under&nbsp;Method,&nbsp;select &quot;Manual&quot;</span></p>
<p><span style="color:#111111">Select:&nbsp;Add</span></p>
<p><span style="color:#111111">Enter the following values into line under Addresses</span></p>
<p><span style="color:#111111">Address: 192.168.40.1</span></p>
<p><span style="color:#111111">Netmask: 255.255.255.0</span></p>
<p><span style="color:#111111">Gateway: 192.168.40.1</span></p>
<p><span style="color:#111111">Close &quot;Editing eth0&quot; dialog box by selecting&quot;Save&quot;</span></p>
<p><span style="color:#111111">Save button is grayed out unless valid values are entered forAddress.</span></p>
<p><a name="t11"></a><strong><span style="color:#111111">6. Close dialog boxNetwork Connections.</span></strong></p>
<p><span style="color:#111111">The Ethernet connection is now configured. The sameconfiguration will be recovered next time</span></p>
<p><span style="color:#111111">the same USB Ethernet adapter is connected.</span></p>
<p><span style="color:#111111">&nbsp;</span></p>
<p><span style="color:#111111">&nbsp;===============================================</span></p>
<p><span style="color:#111111">If you want to use the library &quot;libasound.a/out&quot;,&nbsp;</span><br>
<span style="color:#111111">you may need the &quot;libasound2&quot; package.</span><br>
<br>
<br>
<span style="color:#111111">&nbsp;* For My Nuggets:</span><br>
<span style="color:#111111">&nbsp;* I need library &quot;TLL6527M_C_API_uClinux_TLL2012R1DevTEST&quot; from /prerelease20120823/libary/elfStatic/</span><br>
<span style="color:#111111">&nbsp;* I need include /prerelease2012xxxx/inclue/common/</span><br>
<span style="color:#111111">&nbsp;*</span><br>
<span style="color:#111111">&nbsp;* For Audio:</span><br>
<span style="color:#111111">&nbsp;* I need library &nbsp;&quot;asound&quot; from /opt/ALSA_LIB</span><br>
<span style="color:#111111">&nbsp;* I need include .../prerelease2012xxxx/inclue/audio/</span><br>
<span style="color:#111111">&nbsp;* I need include &nbsp;/opt/ALSA_INCLUDE/alsa/sound</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; I found there is no /opt/ALSA_LIB folder in my Ubuntu12.04 system disk.</span><br>
<br>
=======Note: The brown text below is my hand note for my failed trying. You can just skip it.=======<br>
<span style="font-family:Courier New; color:#cc6600">----so, go to Ubuntu website and download the alsa package:<br>
<br>
<br>
https://launchpad.net/ubuntu/precise/&#43;source/alsa-lib/1.0.25-1ubuntu10<br>
<br>
<br>
Download the alsa-lib_1.0.25.orig.tar.bz2, extract it into Download folder.<br>
run the command in Terminal:<br>
&nbsp; &nbsp; $ cd ~/Download/alsa-lib-1.0.25<br>
&nbsp; &nbsp; $ sudo ./configure<br>
&nbsp; &nbsp; $ sudo make<br>
&nbsp; &nbsp; $ sudo make install<br>
<br>
<br>
There are some errors existed.<br>
-----------------------------<br>
&nbsp; &nbsp; I searched the library &quot;asound&quot;, and I find it is in the &quot;libasound2&quot; package.<br>
----Install &quot;libasound2&quot; package: ----<br>
&nbsp; &nbsp; Download the file &quot;libasound2_1.0.22-0ubuntu7_i386.deb&quot; package from:<br>
&nbsp; &nbsp; http://packages.ubuntu.com/lucid/libasound2<br>
<br>
<br>
&nbsp; &nbsp; Install the .deb package:<br>
&nbsp; &nbsp; &gt; $ sudo dpkg -i libasound2_1.0.22-0ubuntu7_i386.deb<br>
<br>
<br>
&nbsp; &nbsp; An error happend:Package libpython2.6 is not installed.&nbsp;<br>
&nbsp; &nbsp; It depends on libpython2.6.<br>
<br>
<br>
----Install the &quot;libpython2.6&quot; package: ----<br>
&nbsp; &nbsp; Download the package at:http://packages.ubuntu.com/zh-cn/lucid/libpython2.6<br>
<br>
<br>
&nbsp; &nbsp; Install it:<br>
&nbsp; &nbsp; &gt; $ sudo dpkg -i libpython2.6_2.6.5-1ubuntu6.1_i386.deb<br>
<br>
<br>
&nbsp; &nbsp; but:<br>
&nbsp; &nbsp; libpython2.6 depends on python2.6<br>
<br>
<br>
&nbsp; &nbsp; libpython2.6 depends on libssl0.9.8<br>
<br>
<br>
----Install the &quot;python2.6&quot; and &quot;libssl0.9.8&quot;&nbsp;<br>
&nbsp; &nbsp; ----Install Python 2.6 in Ubuntu 12.04:<br>
&nbsp; &nbsp; &nbsp; &nbsp; Ref:http://www.ubuntututorials.com/install-python-2-6-ubuntu-12-04/<br>
&nbsp; &nbsp; &nbsp; &nbsp; Ubuntu 12.04 includes Python 2.7.3 and &nbsp;Python 2.6 is no longer available for install.If you need&nbsp;<br>
<br>
<br>
to run &nbsp;legacy software which only support Python 2.6.Below steps will show you how to install Python 2.6&nbsp;<br>
<br>
<br>
from PPA,alternatively you also can build it yourself.<br>
<br>
<br>
&nbsp; &nbsp; &nbsp; &nbsp; sudo add-apt-repository ppa:fkrull/deadsnakes<br>
&nbsp; &nbsp; &nbsp; &nbsp; sudo apt-get update<br>
&nbsp; &nbsp; &nbsp; &nbsp; sudo apt-get install python2.6 python2.6-dev<br>
<br>
<br>
&nbsp; &nbsp; &nbsp; &nbsp; some error happens, about dependency.<br>
<br>
<br>
&nbsp; &nbsp; ----Install libssl0.9.8 ----<br>
&nbsp; &nbsp; &nbsp; &nbsp; Download it from:https://launchpad.net/ubuntu/precise/i386/libssl0.9.8/0.9.8o-7ubuntu3.1<br>
&nbsp; &nbsp; &nbsp; &nbsp; Install it by dpkg -i libssl...deb<br>
<br>
<br>
&nbsp; &nbsp; &nbsp; &nbsp; It seems like succeeded.<br>
<br>
<br>
&nbsp; &nbsp; ----Install Python 2.6 again----<br>
&nbsp; &nbsp; &nbsp; &nbsp; sudo add-apt-repository ppa:fkrull/deadsnakes<br>
&nbsp; &nbsp; &nbsp; &nbsp; sudo apt-get update<br>
&nbsp; &nbsp; &nbsp; &nbsp; sudo apt-get install python2.6 python2.6-dev<br>
<br>
<br>
&nbsp; &nbsp; &nbsp; &nbsp; I got:&quot;E: Unmet dependencies.&quot;<br>
<br>
<br>
&nbsp; &nbsp; ----Install libpython2.6 :<br>
&nbsp; &nbsp; &nbsp; &nbsp; &gt; sudo dpkg -i libpython2.6_2.6.5-1ubuntu6.1_i386.deb&nbsp;<br>
&nbsp; &nbsp; &nbsp; &nbsp; it still need for python 2.6<br>
<br>
<br>
&nbsp; &nbsp; ----Install Python2.6:<br>
&nbsp; &nbsp; &nbsp; &nbsp; 1. Download &quot;Python-2.6.tar.bz2&quot; from:<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;http://www.python.org/ftp/python/2.6/Python-2.6.tar.bz2<br>
&nbsp; &nbsp; &nbsp; &nbsp; 2. Extract it<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;$ tar jxvf Python-2.6.tar.bz2<br>
&nbsp;<br>
&nbsp; &nbsp; &nbsp; &nbsp; 3. $ cd Python-2.6<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;$ sudo ./configure<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;$ sudo make<br>
<span style="white-space:pre"></span>&nbsp; $ sudo make install<br>
<br>
<br>
&nbsp; &nbsp; ----Install libpython2.6 :<br>
&nbsp; &nbsp; &nbsp; &nbsp; &gt; cd ~/Downloads<br>
&nbsp; &nbsp; &nbsp; &nbsp; &gt; sudo dpkg -i libpython2.6_2.6.5-1ubuntu6.1_i386.deb<br>
&nbsp; &nbsp; &nbsp; &nbsp; Error: libpython2.6 depends on python2.6 (= 2.6.5-1ubuntu6.1);&nbsp;<br>
&nbsp; &nbsp; &nbsp; &nbsp; however: Version of python2.6 on system is 2.6.8-2&#43;precise1.<br>
<br>
<br>
&nbsp; &nbsp; ----Install python2.6.5-1ubuntu6.1<br>
&nbsp; &nbsp; &nbsp; &nbsp; Download it from:<br>
&nbsp; &nbsp; &nbsp; &nbsp; https://launchpad.net/ubuntu/&#43;archive/primary/&#43;files/python2.6_2.6.5.orig.tar.gz<br>
<br>
<br>
&nbsp; &nbsp; &nbsp; &nbsp; Extract: $ tar zxvf python2.6_2.6.5.orig.tar.gz<br>
&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cd ~/Downloads/python2.6-2.6.5<br>
&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ ./configure<br>
&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ sudo make<br>
&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ sudo make install<br>
&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<br>
&nbsp; &nbsp; ----Install libpython2.6 :<br>
&nbsp; &nbsp; &nbsp; &nbsp; &gt; cd ~/Downloads<br>
&nbsp; &nbsp; &nbsp; &nbsp; &gt; sudo dpkg -i libpython2.6_2.6.5-1ubuntu6.1_i386.deb<br>
&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<br>
&nbsp; &nbsp; &nbsp; &nbsp; Error: libpython2.6 depends on python2.6 (= 2.6.5-1ubuntu6.1);&nbsp;<br>
&nbsp; &nbsp; &nbsp; &nbsp; however: Version of python2.6 on system is 2.6.8-2&#43;precise1.<br>
<br>
<br>
&nbsp; &nbsp; ----Install libpython2.6_2.6.8-2&#43;precise1_i386.deb &nbsp;<br>
&nbsp; &nbsp; &nbsp; &nbsp; Download it from:<br>
&nbsp; &nbsp; &nbsp; &nbsp; https://launchpad.net/~fkrull/&#43;archive/deadsnakes/&#43;build/3802469/&#43;files/libpython2.6_2.6.8-<br>
<br>
<br>
2%2Bprecise1_i386.deb<br>
<br>
<br>
&nbsp; &nbsp; &nbsp; &nbsp; Install it:<br>
&nbsp; &nbsp; &nbsp; &nbsp; $ sudo dpkg -i libpython2.6_2.6.8-2&#43;precise1_i386.deb<br>
&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<br>
<span style="white-space:pre"></span>It seems like succeeded.<br>
<br>
<br>
&nbsp; &nbsp; ----Install &quot;libasound2&quot; package again:<br>
<br>
<br>
&nbsp; &nbsp; &nbsp; &nbsp; Install the &quot;libasound2&quot; package:<br>
&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ sudo dpkg -i libasound2_1.0.22-0ubuntu7_i386.deb<br>
&nbsp; &nbsp;&nbsp;<br>
&nbsp; &nbsp; &nbsp; &nbsp; It seems like succeeded.<br>
<br>
<br>
----Check the /opt/ALSA_LIB and /opt/ALSA_INCLUDE<br>
&nbsp; &nbsp; &nbsp; &nbsp; No! :(<br>
<br>
<br>
&nbsp; &nbsp; There is no folder about alsa in /opt, but check if my uClinux programe compiling can be correct. add&nbsp;<br>
</span><span style="color:rgb(204,102,0); font-family:'Courier New'">the &quot;asound&quot; library directly as -lasound, but no assigning the path of it because I have installed the&nbsp;</span><span style="font-family:Courier New; color:#cc6600"><br>
</span><span style="color:rgb(204,102,0); font-family:'Courier New'">libasound2 package above.</span><span style="font-family:Courier New; color:#cc6600"><br>
<br>
<br>
&nbsp; &nbsp; Go back to Eclipse, and configure the library:&nbsp;<br>
&nbsp; &nbsp; &nbsp; &nbsp; asound<br>
&nbsp; &nbsp; &nbsp; &nbsp; &quot;TLL6527M_C_API_uClinux_TLL2012R1DevTEST&quot; from /prerelease20120823/libary/elfStatic/<br>
<br>
<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; configure the include:&nbsp;<br>
&nbsp; &nbsp; &nbsp; &nbsp; prerelease../include/audio<br>
&nbsp; &nbsp; &nbsp; &nbsp; prerelease../include/common<br>
&nbsp; &nbsp; &nbsp; &nbsp; usr/include/alsa/sound<br>
<br>
<br>
&nbsp; &nbsp; When compiling in Eclipse, Get the error: cannot find the lib: -lasound&nbsp;<br>
<br>
<br>
&nbsp;<br>
----Install the libasound2-dev<br>
&nbsp; &nbsp; &gt; $ sudo apt-get install libasound2-dev<br>
&nbsp; &nbsp; Get error:<br>
&nbsp; &nbsp; E: Unmet dependencies. Try 'apt-get -f install' with no packages (or specify a solution).<br>
&nbsp; &nbsp;&nbsp;<br>
&nbsp; &nbsp;&nbsp;<br>
----Install the alsa-lib-1.0.13:<br>
&nbsp; &nbsp; Go to the webpage:<br>
&nbsp; &nbsp; http://www.linuxfromscratch.org/blfs/view/6.3/multimedia/alsa-lib.html<br>
&nbsp; &nbsp; Download the .tar.bz2 package:<br>
&nbsp; &nbsp; http://gd.tuwien.ac.at/opsys/linux/alsa/lib/alsa-lib-1.0.13.tar.bz2<br>
&nbsp; &nbsp; Extract it:<br>
&nbsp; &nbsp; &gt; $ tar jxvf alsa-lib-1.0.13.tar.bz2<br>
&nbsp; &nbsp; &gt; $ cd ~/Downloads/alsa-lib-1.0.13/<br>
&nbsp; &nbsp;&nbsp;<br>
&nbsp; &nbsp; Install the library:<br>
&nbsp; &nbsp; &gt; $ ./configure --enable-static &amp;&amp; make<br>
&nbsp; &nbsp; &gt; $ sudo make install<br>
<br>
<br>
&nbsp; &nbsp;&nbsp;<br>
<br>
<br>
&nbsp; &nbsp; It seems that installing the alsa-lib-1.0.13 succeeded,&nbsp;<br>
&nbsp; &nbsp; the library file &quot;libasound.a/out&quot; at: &nbsp;/usr/lib<br>
&nbsp; &nbsp; I just included in Eclipse the header files at: &nbsp;/usr/include/alsa/sound<br>
<br>
<br>
&nbsp; &nbsp; But the Eclipse still report error:<br>
&nbsp; &nbsp; &nbsp; &nbsp; DescriptionResource<span style="white-space:pre"></span>PathLocation<span style="white-space:pre"></span>Type<br>
&nbsp; &nbsp; &nbsp; &nbsp; skipping incompatible /usr/lib/libasound.so when searching for -lasound<span style="white-space:pre"></span>audio_example<span style="white-space:pre"></span><br>
<br>
<br>
C/C&#43;&#43; Problem<br>
&nbsp; &nbsp; &nbsp; &nbsp; skipping incompatible /usr/lib/libasound.a when searching for -lasound<span style="white-space:pre"></span>audio_example<span style="white-space:pre"></span><span style="white-space:pre"></span><br>
<br>
<br>
C/C&#43;&#43; Problem<br>
&nbsp; &nbsp; &nbsp; &nbsp; cannot find -lasound<span style="white-space:pre"></span>audio_example<span style="white-space:pre"></span><span style="white-space:pre"></span>C/C&#43;&#43; Problem<br>
&nbsp; &nbsp; &nbsp; &nbsp; make: *** [audio_example] Error 1<span style="white-space:pre"></span>audio_example<span style="white-space:pre"></span><span style="white-space:pre"></span>C/C&#43;&#43; Problem</span></p>
<p><span style="font-family:Courier New">================ End of Brown Text ===================<br>
</span><br>
<br>
<span style="color:#111111">----Copy the /opt/ALSA_INCLUDE/alsa/sound &amp; /opt/ALSA_LIB from the Fedora Host System that released by The&nbsp;</span><span style="color:#111111">Learning Labs company. Put them into my U-disk.</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; Now plug in the u-disk, and we need to mount the devices.&nbsp;</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; Run the command to see which devices do you have:&nbsp;</span><br>
<span style="color:#111111">&nbsp; &nbsp; &gt; $ sudo fdisk -l /dev/sda</span><br>
<span style="color:#111111">&nbsp; &nbsp; And I got:</span><br>
<span style="color:#111111">Disk /dev/sda: 8589 MB, 8589934592 bytes</span><br>
<span style="color:#111111">255 heads, 63 sectors/track, 1044 cylinders, total 16777216 sectors</span><br>
<span style="color:#111111">Units = sectors of 1 * 512 = 512 bytes</span><br>
<span style="color:#111111">Sector size (logical/physical): 512 bytes / 512 bytes</span><br>
<span style="color:#111111">I/O size (minimum/optimal): 512 bytes / 512 bytes</span><br>
<span style="color:#111111">Disk identifier: 0x00029a1e</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp;Device Boot &nbsp; &nbsp; &nbsp;Start &nbsp; &nbsp; &nbsp; &nbsp; End &nbsp; &nbsp; &nbsp;Blocks &nbsp; Id &nbsp;System</span><br>
<span style="color:#111111">/dev/sda1 &nbsp; * &nbsp; &nbsp; &nbsp; &nbsp;2048 &nbsp; &nbsp;14680063 &nbsp; &nbsp; 7339008 &nbsp; 83 &nbsp;Linux</span><br>
<span style="color:#111111">/dev/sda2 &nbsp; &nbsp; &nbsp; &nbsp;14682110 &nbsp; &nbsp;16775167 &nbsp; &nbsp; 1046529 &nbsp; &nbsp;5 &nbsp;Extended</span><br>
<span style="color:#111111">/dev/sda5 &nbsp; &nbsp; &nbsp; &nbsp;14682112 &nbsp; &nbsp;16775167 &nbsp; &nbsp; 1046528 &nbsp; 82 &nbsp;Linux swap / Solaris</span><br>
<br>
<br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; There should be a FAT32 System disk, but there is no.</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; If you only plugged in one U-disk, and your disk is not scsi interface, its hardware name is: sda1</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; Build an usb folder at your mnt: $ sudo mkdir /mnt/usb</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; Mount your u-disk: &nbsp;$ mount -t vfat /dev/sda1 &nbsp;/mnt/usb</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; I copied it to the /opt folder from my ~/Downloads which is containing the /ALSA_INCLUDE and /ALSA_LIB&nbsp;</span><br>
<br>
<br>
<span style="color:#111111">which is copied from U-disk:</span><br>
<span style="color:#111111">&nbsp; &nbsp; ~/Downloads/opt&gt; $ sudo cp -r ALSA_INCLUDE /opt/</span><br>
<span style="color:#111111">&nbsp; &nbsp; ~/Downloads/opt&gt; $ sudo cp -r ALSA_LIB /opt/</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; Now we have the libraries and head files needed for audio programming.</span><br>
<span style="color:#111111">&nbsp; &nbsp; Test in Eclipse: I don't have the permission to access the foler I copied just now.</span><br>
<span style="color:#111111">&nbsp; &nbsp; [Here, you can try command: &quot;sudo chmod 777 foldername&quot;]</span><br>
<span style="color:#111111">&nbsp; &nbsp; [Ref:http://superuser.com/questions/208606/how-to-change-file-permissions-for-a-directory-in-one-</span><br>
<br>
<br>
<span style="color:#111111">command]</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; ----Log out linux and log in as the root user:</span><br>
<span style="color:#111111">&nbsp; &nbsp; Ref:http://linuxathena.com/tutorials/login-root-account-gui-ubuntu-12-04/</span><br>
<span style="color:#111111">&nbsp; &nbsp; Open Terminal, input: sudo passwd</span><br>
<span style="color:#111111">&nbsp; &nbsp; update the UNIX password: xxx</span><br>
<span style="color:#111111">&nbsp; &nbsp;&nbsp;</span><br>
<span style="color:#111111">&nbsp; &nbsp; By default, Ubuntu doesn't allow root log on the graphic interface.</span><br>
<span style="color:#111111">&nbsp; &nbsp; Modify the &quot;/etc/gdm/gdm.conf&quot; file to allow root log on.</span><br>
<span style="color:#111111">&nbsp; &nbsp; In this file, modify the &quot;AllowRoot = false&quot; into &quot;AllowRoot = true&quot;.</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; Ubuntu 12.04下允许Root用户直接登录图形界面方法:</span><br>
<span style="color:#111111">&nbsp; &nbsp; Ref:http://askubuntu.com/questions/126286/unable-to-login-as-root-from-the-login-screen-even-though-</span><br>
<br>
<br>
<span style="color:#111111">i-have-a-root-account</span><br>
<span style="color:#111111">&nbsp; &nbsp; Now, I'm using Ubuntu 12.04, I have to enable it in /etc/lightdm/lightdm.conf</span><br>
<span style="color:#111111">&nbsp; &nbsp; Enter the sudo mode:</span><br>
<span style="color:#111111">&nbsp; &nbsp; &gt; $ su root</span><br>
<span style="color:#111111">&nbsp; &nbsp; Change the file permission of &quot;lightdm.conf&quot;</span><br>
<span style="color:#111111">&nbsp; &nbsp; &gt; # chmod 777 /etc/lightdm/lightdm.conf</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; &gt; # vi lightdm.conf</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; Add the following line:</span><br>
<span style="color:#111111">&nbsp; &nbsp; greeter-show-manual-login = true</span><br>
<span style="color:#111111">&nbsp; &nbsp; Save and exit: &quot;:wq&quot;</span><br>
<span style="color:#111111">&nbsp; &nbsp; Reboot the system. &nbsp;Select the user, input &quot;root&quot; and password.</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; Change the permission of /opt/alsa* folders.</span><br>
<span style="color:#111111">&nbsp; &nbsp; Logout and login as former user.</span><br>
<span style="color:#111111">&nbsp; &nbsp;&nbsp;</span><br>
<span style="color:#111111">&nbsp; &nbsp; Test the access to the /opt/alsa* folders in Eclipse.</span><br>
<span style="color:#111111">&nbsp; &nbsp; There are 5 new errors occured.</span><br>
<span style="color:#111111">&nbsp; &nbsp; Description</span><span style="color:rgb(17,17,17); white-space:pre"></span><span style="color:#111111">Resource<span style="white-space:pre"></span>Path</span><span style="color:rgb(17,17,17); white-space:pre"></span><span style="color:#111111">Location<span style="white-space:pre"></span>Type</span><br>
<span style="color:#111111">undefined reference to `_dlsym'</span><span style="color:rgb(17,17,17); white-space:pre"></span><span style="color:#111111">audio_example<span style="white-space:pre"></span>line 121, external location: /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/dlmisc.c<span style="white-space:pre"></span>C/C&#43;&#43;
 Problem</span><br>
<span style="color:#111111">&nbsp; &nbsp; undefined reference to `_dlclose'</span><span style="color:rgb(17,17,17); white-space:pre"></span><span style="color:#111111">audio_example<span style="white-space:pre"></span>line 91, external location: /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/dlmisc.c<span style="white-space:pre"></span>C/C&#43;&#43;
 Problem</span><br>
<span style="color:#111111">&nbsp; &nbsp; undefined reference to `_dlsym'</span><span style="color:rgb(17,17,17); white-space:pre"></span><span style="color:#111111">audio_example<span style="white-space:pre"></span>line 167, external location: /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/dlmisc.c<span style="white-space:pre"></span>C/C&#43;&#43;
 Problem</span><br>
<span style="color:#111111">&nbsp; &nbsp; make: *** [audio_example] Error 1</span><span style="color:rgb(17,17,17); white-space:pre"></span><span style="color:#111111">audio_example<span style="white-space:pre"></span><span style="white-space:pre"></span>C/C&#43;&#43; Problem</span><br>
<span style="color:#111111">undefined reference to `_dlopen'</span><span style="color:rgb(17,17,17); white-space:pre"></span><span style="color:#111111">audio_example<span style="white-space:pre"></span>line 70, external location: /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/dlmisc.c<span style="white-space:pre"></span>C/C&#43;&#43;
 Problem</span><br>
<br>
<br>
<span style="color:#111111">&nbsp; &nbsp; And I received the notes form the TLL board provider:</span><br>
<span style="color:#111111">----------------------</span><br>
<span style="color:#999999">At this moment we're only supporting our Fedora based TLL SDE as Design / Development host OS for TLL platforms, this enables us to ensure all the tools/settings/dependencies work seamlessly with each other and thus requires us to
 concentrate efforts on a single platform avoiding a proliferation of different OS flavors. Hence we've tested the nuggets and build tools ONLY on TLL SDE(Fedora).<br>
<br>
<br>
I would guess that ALSA libs would not be OS flavor dependent and might be usable just by coping library binaries and include header files over to your Ubuntu machine. However we'll not be able to support any issues you might face there.<br>
<br>
<br>
Why don't you consider building a personal TLL SDE bootable pen drive which you can plug into your own machine while working with TLL platforms and then just reboot your system into your ubuntu when you need to use other things you are using. You can install
 it on a 4GB (or greater) pen drive.<br>
<br>
<br>
Here is a link to SDE installation notes:<br>
https://tll.assembla.com/spaces/designcenter/wiki/System_Design_Environment#2._installation_of_tll_sde<br>
<br>
<br>
Please feel free to let us know any issues you face with this use mode of SDE.<br>
<br>
<br>
&nbsp;---------<br>
Hello ,<br>
<br>
<br>
let me echo his words.<br>
<br>
<br>
I would not recommend any use on a custom installation. There are too many variables that can not be sufficiently controlled. As such it is practically infeasible to support such an installation.<br>
<br>
<br>
Alternatively, you can install/run the SDE directly as outlined by Ashish, however at a loss of company-specific customizations.</span><br>
<span style="color:#111111">&nbsp; &nbsp;&nbsp;</span><br>
<span style="color:#111111">&nbsp; &nbsp; ----------------------------</span></p>
<p><span style="color:#111111"><br>
</span></p>
<p><span style="color:#111111">至此，我配置好了blackfin的开发环境，但是没法配置适用于TLL所有板载功能的开发环境。因为TLL板自己的开发环境为了使用TLL板，有太多的东西需要配置。我并没有完整的配置文档。或许他们公司经过一次次的修改，才发布的SDE本身已经不知道要配置哪些东西了。</span></p>
<p><span style="color:#111111">到此为止吧。。。。。</span><span style="color:#cc0000"><strong>但是</strong>：</span></p>
<p><span style="color:#111111">=============================================================================<br>
&nbsp; &nbsp;&nbsp;</span><br>
<strong><span style="color:#ff0000">&nbsp; &nbsp; But when I compile the accelerometer, there is no errors happened. This makes me burn a new fire in my mind.</span></strong></p>
<p><span style="color:#111111"><strong>&nbsp; &nbsp; I began to test the other nuggets except audio.</strong></span></p>
<p><span style="color:#111111">=============================================================================</span></p>
<p><span style="color:#111111">&nbsp; &nbsp;&nbsp;&nbsp; &nbsp; OMG!!!<br>
&nbsp; &nbsp; I fixed this problem by: add the libarary -ldl into my objects.mk file which is created by Eclipse.</span></p>
<p><span style="color:rgb(17,17,17)">&nbsp; &nbsp; so:</span></p>
<p><span style="color:rgb(17,17,17)"></span></p>
<pre name="code" class="cpp">   /* For My Nuggets On Ubuntu12.04:
    * You need library  &quot;TLL6527M_C_API_uClinux_TLL2012R1DevTEST&quot; at /prerelease20120823/libary/elfStatic/
    * You need include  /prerelease2012xxxx/inclue/common/
    *
    * For Audio:
    * You need library  &quot;dl&quot; (No need to assign its path, system default.)
    * You need library  &quot;asound&quot; from /opt/ALSA_LIB
    * You need include  .../prerelease2012xxxx/inclue/audio/
    * You need include  /opt/ALSA_INCLUDE/alsa/sound
    */</pre><br>
<br>
&nbsp; &nbsp;&nbsp;<br>
=====================================================<br>
遇到 Eclipse 编译时出现：<br>
undefined reference to '_dlclose'<br>
undefined reference to '_dlopen'<br>
undefined reference to '_dlsym'<br>
undefined reference to '_dlsym'<br>
我的解决办法：<br>
&nbsp; &nbsp; 我是修改了.mk 文件，实际上是增加了一个库， dl (dynamic link)&nbsp;<br>
&nbsp; &nbsp; 在原本描述链接库的地方:
<p></p>
<pre name="code" class="plain">LIBS := -lTLL6527M_C_API_uClinux_TLL2012R1DevTEST -lasound</pre>&nbsp; &nbsp; 修改为:&quot;<pre name="code" class="plain">LIBS := -lTLL6527M_C_API_uClinux_TLL2012R1DevTEST -lasound -ldl</pre>再重新编译，就没有错误产生了。<br>
<p>===================================================== &nbsp; &nbsp;</p>
<p>&nbsp; &nbsp;&nbsp;But I still met the errors sent from board when I was launching the audio app on the board:</p>
<p>&nbsp; &nbsp; But I met the errors when I launch the app bin file on my target board! :(</p>
<p>&nbsp; &nbsp; See the log below:</p>
<p></p>
<pre name="code" class="plain">root:/home&gt; ls
audio_example  matlab-serial
root:/home&gt; ./audio_example 
Switching to default type SSM2603, for stereo capture 
through Line In, at 16 bits per sample and 48000 Hz 
sampling rate
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/dlmisc.c:246:(snd1_dlobj_cache_get) symbol _snd_ctl_hw_open is not defined inside [builtin]
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/confmisc.c:674:(snd_determine_driver) could not open control for card 0
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/conf.c:3326:(snd_config_hooks_call) function snd_config_hook_load_for_all_cards returned error: No such device or address
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/dlmisc.c:246:(snd1_dlobj_cache_get) symbol _snd_ctl_hw_open is not defined inside [builtin]
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/confmisc.c:674:(snd_determine_driver) could not open control for card 0
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/conf.c:4184:(_snd_config_evaluate) function snd_func_card_driver returned error: No such device or address
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/confmisc.c:392:(snd_func_concat) error evaluating strings
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/conf.c:4184:(_snd_config_evaluate) function snd_func_concat returned error: No such device or address
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/confmisc.c:1251:(snd_func_refer) error evaluating name
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/conf.c:4184:(_snd_config_evaluate) function snd_func_refer returned error: No such device or address
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/conf.c:4663:(snd_config_expand) Evaluate error: No such device or address
ALSA lib /home/Lavanya/2011R1_Release/blackfin-linux-dist_RC3/lib/alsa-lib/alsa-lib-1.0.24/src/pcm/pcm.c:2212:(snd_pcm_open_noupdate) Unknown PCM cards.pcm.default
Cannot open device::No such device or address
Cannot Init
root:/home&gt;</pre><br>
&nbsp; &nbsp; I really do not know how to fix the alsa-lib problem....
<p></p>
<p><span style="color:#111111"><br>
</span></p>
<p><span style="color:#111111"><br>
</span></p>
<p><span style="color:rgb(17,17,17)">&nbsp;</span></p>
<p><span style="color:rgb(17,17,17)">&nbsp;</span></p>
<p><span style="color:#111111">================================================</span></p>
<p><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; </span><span style="color:#111111">参考：</span><span style="color:#111111"><a href="http://blog.csdn.net/cp1300/article/category/1209321" target="_blank"><span style="color:#336699">【嵌入式Linux</span></a></span><span style="color:#336699">开发环境配置】</span></p>
<p>&nbsp;</p>
   

---
title: '【技术】Ubuntu上位机配置Blackfin开发环境手记'
date: 2013-01-02 19:26:00
---


<p align="left" style="font-size:14px; line-height:21px"><span style="line-height:1.5; font-size:12pt"><span style="font-family:Arial">Ubuntu配置上位机<span lang="EN-US" style="line-height:1.5">Blackfin</span>开发环境手记：<span lang="EN-US" style="line-height:1.5">[</span>参考<span lang="EN-US" style="line-height:1.5">]<a href="http://blog.csdn.net/cp1300/article/details/8205282" target="_blank">http://blog.csdn.net/cp1300/article/details/8205282</a></span></span></span></p>
<p align="left" style="line-height:21px"><span style="font-family:Arial; font-size:24px; color:#464646">My note for configuring the Software Development Environment(SDE) for TLL board on Ubuntu 12.04</span></p>
<p align="center" style="text-align:left; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px">
<strong><span style="font-family:宋体; line-height:1.5; font-size:22pt">【</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:22pt">Section 0: Working Aim</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:22pt">工作目的】</span></strong></p>
<p align="left" style="font-size:14px; line-height:21px"><span style="line-height:1.5; font-size:12pt"><span style="font-family:Arial">现在手里有一个嵌入式开发板，搭载的是&nbsp;<span lang="EN-US" style="line-height:1.5">AnalogDevices</span>&nbsp;公司的&nbsp;<span lang="EN-US" style="line-height:1.5">Blackfin527
 Processor</span>，预装板载<span lang="EN-US" style="line-height:1.5">Uclinux</span>操作系统。现在要配置上位机<span lang="EN-US" style="line-height:1.5">Linux/Ubuntu12.04</span>&nbsp;的开发环境，包括<span lang="EN-US" style="line-height:1.5">Blackfin Toolchain</span>，<span lang="EN-US" style="line-height:1.5">Eclipse</span>开发环境，和<span lang="EN-US" style="line-height:1.5">Library.</span></span></span></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px">
<span lang="EN-US" style="font-family:宋体; line-height:1.5; font-size:12pt">&nbsp;</span></p>
<p align="center" style="text-align:left; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px">
<strong><span style="font-family:宋体; line-height:1.5; font-size:22pt">【</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:22pt">Section1: Install Toolchain</span><span style="font-family:宋体; line-height:1.5; font-size:22pt">】</span></strong></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin:12pt 0cm 4.8pt; line-height:14.4pt">
<strong><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:12pt; color:rgb(17,17,17)">Install Blackfin Toolchain</span></strong></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin:9.6pt 0cm 3pt; line-height:14.4pt">
<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">Refer to the Note after this section if you meet some issue during this installation.</span></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin:9.6pt 0cm 3pt; line-height:14.4pt">
<strong><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:12pt; color:rgb(17,17,17)">1.1 Package Installation (recommended)</span></strong></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Get
 the latest Blackfin Toolchain from&nbsp;<a href="http://blackfin.uclinux.org/gf/project/toolchain/frs/"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">ADI</span></a></span></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Current
 up to date version is: 2011R1-RC4</span></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Example
 download command:</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">&gt; wget<a href="http://blackfin.uclinux.org/gf/download/frsrelease/531/9508/blackfin-toolchain-2011R1-RC4.i386.rpm"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">http://blackfin.uclinux.org/gf/download/frsrelease/531/9508/blackfin-toolchain-2011R1-RC4.i386.rpm</span></a>&nbsp;</span></p>
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">&gt; wget<a href="http://blackfin.uclinux.org/gf/download/frsrelease/531/9512/blackfin-toolchain-elf-gcc-4.3-2011R1-RC4.i386.rpm"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">http://blackfin.uclinux.org/gf/download/frsrelease/531/9512/blackfin-toolchain-elf-gcc-4.3-2011R1-RC4.i386.rpm</span></a></span></p>
</div>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Check
 if a previous toolchain is already installed</span></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">If
 so, remove old tool chain using rpm -e ...</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">&gt; rpm -qa blackfin-toolchain\*</span></p>
</div>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Install
 Toolchain using RPM.</span></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Note:
 If a previous toolchain is installed, you may need to uninstall the previous toolchain see above</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">rpm -ivh blackfin-toolchain-*</span></p>
</div>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">More details on toolchain installation can be found at ADI's&nbsp;<a href="http://docs.blackfin.uclinux.org/doku.php?id=toolchain:installing"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">installation
 wiki entry.</span></a></span></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin:9.6pt 0cm 3pt; line-height:14.4pt">
<strong><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:12pt; color:rgb(17,17,17)">1.2 Source Installation (advanced)</span></strong></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">Prequisites</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">yum install automake autoconf ncurses-devel zlib-devel texinfo</span></p>
</div>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">set
 desired version</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">set VERSION=2011R1-RC2</span></p>
</div>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">Get
 the latest Version of the GNU toolchain</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">svn checkout svn://sources.blackfin.uclinux.org/toolchain/tags/$VERSION/</span></p>
</div>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; text-indent:0cm; margin-left:0cm; line-height:13.65pt">
<span lang="EN-US" style="font-family:symbol; line-height:1.5; font-size:10pt; color:rgb(17,17,17)">·<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">Compile
 and Install</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">&gt; cd bfin_toolchain.$VERSION/buildscript/ &gt; ./BuildToolChain -o &lt;COMPILER_NSTALLATION_DIR&gt;</span></p>
</div>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:15.75pt; margin-bottom:7.5pt; background-color:rgb(255,252,246)">
<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">Note:</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">这里要注意的是：</span></p>
<p align="left" style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:15.75pt; margin-bottom:7.5pt; background-color:rgb(255,252,246)">
<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">1.ADI offers serious packages of toolchain of one version for different usages. You should select which one is needed.&nbsp; I downloaded the 3 packages:</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">&nbsp; &gt; blackfin-toolchain-2012R2-RC1.i386.rpm</span></p>
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">&nbsp; &gt; blackfin-toolchain-elf-gcc-4.3-2012R2-RC1.i386.rpm</span></p>
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">&nbsp; &gt; blackfin-toolchain-uclibc-default-2012R2-RC1.i386.rpm</span></p>
</div>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;from the Blackfin Linux website in the toolchain file release page</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">&nbsp;</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">2. Ubuntu does not support installing this toolchain by RPM tool.</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">&nbsp; &gt; I used the &quot;How to Install rpm package on Ubuntu&quot; approach to go around it. Chinese tutorial:</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">注意，用</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">alien</span><span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">转换的</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">deb</span><span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">包并不能保证</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">100%</span><span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">顺利安装，所以可以找到</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">deb</span><span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">最好直接用</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">deb</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">&nbsp;&nbsp;&nbsp;&nbsp;</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">有时候，我们想要使用的软件并没有被包含到</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">Ubuntu</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">的仓库中，而程序本身也没有提供让</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">Ubuntu</span><span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">可以使用的</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">deb</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">包，你又不愿从源代码编译。但假如软件提供有</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">rpm</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">包的话，我们也是可以在</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">Ubuntu</span><span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">中安装的。</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">方法一：</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">1.</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">先安装</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">alien</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">和</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">fakeroot</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">这两个工具，其中前者可以将</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">rpm</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">包转换为</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">deb</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">包。安装命令为：</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">Intall
 the &quot;alien&quot; and &quot;fakeroot&quot; tools:<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:15.75pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt">$&nbsp;</span><span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">sudo apt-get install alien fakeroot</span></p>
</div>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">2.</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">将需要安装的</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">rpm</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">包下载备用，假设为</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">package.rpm</span><span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">。</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">Assume
 your package is: package.rpm<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:11pt; color:rgb(70,70,70)"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
3.</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">使用</span>&nbsp;<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">alien</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">将</span>&nbsp;<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">rpm</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">包转换为</span>&nbsp;<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">deb</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">包：</span>&nbsp;<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">use
 alien to convert rpm package into deb package:</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:15.75pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">$&nbsp;fakeroot alien package.rpm</span></p>
</div>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">4.</span>&nbsp;<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">一旦转换成功，我们可以即刻使用以下指令来安装：</span>&nbsp;<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">After
 a while, if succeed, install it using command:</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:15.75pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:11pt; color:rgb(70,70,70)">$ sudo dpkg -i package.deb</span></p>
</div>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">如何转换</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">rpm</span><span style="font-family:宋体; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">详细教程，参考这里</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11pt; color:rgb(17,17,17)">:&nbsp;<a href="http://zhidao.baidu.com/question/377062512">http://zhidao.baidu.com/question/377062512</a></span></p>
<p align="center" style="text-align:left; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px; margin:12pt 0cm 4.8pt">
<strong><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:16.5pt; color:rgb(17,17,17)">[Section2. Install Eclipse]</span></strong></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Follow OS independent instructions:<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
Generally, follow instructions at:&nbsp;<a href="http://docs.blackfin.uclinux.org/doku.php?id=toolchain:eclipse:install"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">link</span></a></span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Note: It seems that Blackfin Toolchain only works in Helios Eclipse</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Install components:&nbsp;</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; text-indent:17.25pt; line-height:13.65pt">
<span style="font-family:宋体; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">•</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">&nbsp;&nbsp;&nbsp;&nbsp; Blackfin ucLinux</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; text-indent:17.25pt; line-height:13.65pt">
<span style="font-family:宋体; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">•</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">&nbsp;&nbsp;&nbsp;&nbsp; GDB Hardware Debugging</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; text-indent:17.25pt; line-height:13.65pt">
<span style="font-family:宋体; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">•</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">&nbsp;&nbsp;&nbsp;&nbsp; Target Terminal Management</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; text-indent:17.25pt; line-height:13.65pt">
<span style="font-family:宋体; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">•</span><span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">&nbsp;&nbsp;&nbsp;&nbsp; ZynxCDT</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Below is my steps:</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span lang="EN-US" style="font-family:Tahoma,sans-serif; line-height:1.5; font-size:11.5pt; color:rgb(17,17,17)">Refer to this web page: http://docs.blackfin.uclinux.org/doku.php?id=toolchain:eclipse:install</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:15.75pt; margin-top:6pt">
<strong><span lang="EN-US" style="font-family:Verdana,sans-serif; line-height:1.5; font-size:12pt; color:rgb(70,70,70)">&nbsp;&nbsp;&nbsp;2.1.Install the Java Runtime Environment (JRE)</span></strong></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:15.75pt; margin-top:6pt">
<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">Checking your Java Runtime Environment version:&nbsp;</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5">$ java -version</span></p>
</div>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:15.75pt; margin-top:6pt">
<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">If you do not have Java installed, go through the :<a href="http://www.64bitjungle.com/ubuntu/install-java-jre-160-update-x-on-hardy-as-the-default-java-runtime/">Install
 Java Runtime Env. 1.6.0</a></span><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:15.75pt; margin-top:6pt">
<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">After you finished, the lines below should be displayed:</span><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; color:rgb(70,70,70)"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:14.25pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5">$ java -version<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
java version &quot;1.6.0_16&quot;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
Java(TM) SE Runtime Environment (build 1.6.0_16-b01)<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
Java HotSpot(TM) Client VM (build 14.2-b01, mixed mode, sharing)</span></p>
</div>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:15.75pt; margin-top:6pt">
<strong><span lang="EN-US" style="font-family:Verdana,sans-serif; line-height:1.5; font-size:12pt; color:rgb(70,70,70)">&nbsp;</span></strong></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:15.75pt; margin-top:6pt">
<strong><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;2.2 Install the IDDE (Eclipse)</span></strong></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:15.75pt; margin-top:6pt">
<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">There are no executable installers for Eclipse. It is distributed as a zip archive which you can unpack anywhere you like, and then just run the&nbsp;eclipse.exe&nbsp;program
 in the eclipse sub-directory.</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:15.75pt; margin-top:6pt">
<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">To get the installer, visit&nbsp;the&nbsp;<a href="http://www.eclipse.org/downloads/" target="_blank">Eclipse download page.</a></span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<span style="font-family:Arial,sans-serif; line-height:15.75pt; color:rgb(70,70,70)">You will be presented with a variety of download choices. The only difference between them is the default plug-in set. Since you will most likely be compiling/debugging code
 for the Blackfin processor, you should pick&nbsp;</span><span style="color:rgb(70,70,70); font-family:Arial,sans-serif; line-height:15.75pt">Eclipse&nbsp;IDE&nbsp;for C/C&#43;&#43; Developers</span><span style="font-family:Arial,sans-serif; line-height:15.75pt; color:rgb(70,70,70)">.:&nbsp;</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; margin-top:6pt; line-height:13.65pt">
<img src="http://docs.blackfin.uclinux.org/lib/exe/fetch.php?media=toolchain:eclipse:junoeclipseidecpp.png" name="image_operate_17811355807712359" alt="" style="border:0px; line-height:13.65pt; max-width:690px"></p>
<div>
<div style="font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif">
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;Note: It seems that Blackfin Toolchain only works in&nbsp;<a href="http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/helios/SR2/eclipse-cpp-helios-SR2-linux-gtk.tar.gz" target="_blank">Helios
 Version</a>&nbsp;(Download this verion)</span></p>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">Extract: eclipse-cpp-helios-SR2-linux-gtk.tar.gz ,&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.Double-Click on Archive and Extract Eclipse into /tmp</span></p>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.Open&nbsp;Terminal&nbsp;Window</span></p>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.Install Required Packages</span></p>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;</span></p>
<div style="line-height:1.5; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:15.75pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5">$&nbsp;</span><span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:12pt; color:rgb(70,70,70)">sudo su</span><span lang="EN-US" style="font-family:宋体; line-height:1.5; font-size:12pt; color:rgb(70,70,70)"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
# apt-get install g&#43;&#43;</span></p>
</div>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;</span></p>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;4.Relocate Eclipse</span></p>
<div style="line-height:1.5; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:15.75pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:12pt; color:rgb(70,70,70)">#&nbsp;mv /tmp/eclipse /opt</span></p>
</div>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;5.Create a Symlink</span></p>
<div style="line-height:1.5; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:15.75pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:12pt; color:rgb(70,70,70)">#&nbsp;ln -s /opt/eclipse/eclipse /usr/bin/eclipse</span></p>
</div>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">6. After, you can Start Eclipse from Terminal simply with:</span></p>
<div style="line-height:1.5; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:15.75pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:12pt; color:rgb(70,70,70)">#&nbsp;eclipse</span></p>
</div>
<p style="margin:6pt 0cm 12pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">or: (the way by which we launch Eclipse is under the Super User permision)</span></p>
<div style="line-height:1.5; border:1pt dashed rgb(187,187,187); padding:4pt; background-color:rgb(247,249,250)">
<p align="left" style="margin:7.5pt 0cm; line-height:15.75pt; border:none; padding:0cm">
<span lang="EN-US" style="font-family:'Courier new'; line-height:1.5; font-size:12pt; color:rgb(70,70,70)">$ exit<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
$ sudo eclipse</span></p>
</div>
<p style="margin:6pt 0cm 12pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If you want to overcome the root permission issue, refer to:&nbsp;<a href="http://jonathan.lalou.free.fr/?p=1310" target="_blank"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">here</span></a>.&nbsp;but
 it does not work on my Ubuntu.</span></p>
<p style="margin:6pt 0cm 12pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;</span></p>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Reference:&nbsp;<a href="http://install-climber.blogspot.com/2012/10/installEclipseJunoCPPUbuntu1210Quantal.html" target="_blank"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">How-to
 Install Eclipse Juno 4.2 C/C&#43;&#43; on Ubuntu</span></a><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(17,17,17)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If Installed Eclipse on Linux, but it does not start,</span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; color:rgb(70,70,70)"><a href="http://wiki.eclipse.org/IRC_FAQ#I_just_installed_Eclipse_on_Linux.2C_but_it_does_not_start._What_is_the_problem.3F" title="IRC FAQ"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">read
 this</span></a><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; color:rgb(70,70,70)"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span><strong><span lang="EN-US" style="font-family:Verdana,sans-serif; line-height:1.5; font-size:12pt; color:rgb(70,70,70)">&nbsp;&nbsp;&nbsp;&nbsp;2.3 Install the&nbsp;Blackfin Plug-ins</span></strong></p>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;</span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(70,70,70)">The
 Blackfin plug-ins will require Eclipse 4.2, as well as the CDT. Additionally, you will have to install the GDB Hardware Debugging CDT plug-in (it is usually not installed by default).&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To install the GDB Hardware Debugging plug-in, the CDT update site needs enabled.&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To enable the CDT site:</span></p>
<p align="left" style="margin-left:0cm; text-indent:0cm; line-height:18pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">1.<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">Select
 the Eclipse menu Window→Preferences.</span></p>
<p align="left" style="margin-left:0cm; text-indent:0cm; line-height:15.75pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; color:rgb(70,70,70)">2.<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">Select
 Install/Update→Available Software Sites and select the site,&nbsp;<a href="http://download.eclipse.org/tools/cdt/releases/juno" target="_blank"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">http://download.eclipse.org/tools/cdt/releases/juno</span></a>.</span></p>
<p align="left" style="line-height:18pt"><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">(Here I used Helios, it's a little different. Keep it Enabled.</span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(70,70,70)">&nbsp;</span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:red">Maybe
 we need to add the link in the last line</span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">)</span></p>
<p align="left" style="margin-left:0cm; text-indent:0cm; line-height:18pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">3.<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">Use
 the Enable button to enable the site.</span></p>
<p style="margin-top:6pt; line-height:15.75pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; color:rgb(70,70,70)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">Install
 the&nbsp;<strong>GDB&nbsp;Hardware</strong>&nbsp;Debugging plug-in via the&nbsp;</span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(70,70,70)"><a href="http://wiki.eclipse.org/FAQ_What_is_the_Update_Manager?" title="http://wiki.eclipse.org/FAQ_What_is_the_Update_Manager?"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">Update
 Manager</span></a></span><span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">:</span></p>
<p align="left" style="margin-left:0cm; text-indent:0cm; line-height:18pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">1.<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">Select
 the Eclipse menu Help→Install New Software</span></p>
<p align="left" style="margin-left:0cm; text-indent:0cm; line-height:18pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">2.<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">Select
 the&nbsp;<a href="http://download.eclipse.org/tools/cdt/releases/indigo" title="http://download.eclipse.org/tools/cdt/releases/indigo"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(62,115,160)">http://download.eclipse.org/tools/cdt/releases/indigo</span></a>&nbsp;site
 in the Work With list(Add this, if there isn't.)</span></p>
<p align="left" style="margin-left:0cm; text-indent:0cm; line-height:18pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">3.<span style="font-family:'Times new roman'; line-height:normal; font-size:7pt">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span>&nbsp;<span lang="EN-US" style="font-family:Arial,sans-serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">Enable
 the&nbsp;GDB&nbsp;Hardware Debugging plugin under CDT Optional Features</span></p>
<p align="left" style="line-height:18pt"><span lang="EN-US" style="font-family:simsun,serif; line-height:1.5; font-size:9pt; color:rgb(69,75,87)">&nbsp;</span></p>
<ol style="margin:0px 0px 1em 3.5em; padding-left:0px">
<li>
<div style="line-height:1.5em; color:rgb(69,75,87); font-size:12px"><span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5em; color:rgb(69,75,87); font-size:12px"><span style="font-family:Arial"><img src="http://docs.blackfin.uclinux.org/lib/exe/fetch.php?media=toolchain:eclipse:junoeclipseidegdbdebugging.png" alt="" name="image_operate_80701355810940925" style="border:0px; display:block; margin:0px auto; font-family:Arial,Helvetica,sans-serif; line-height:normal"></span></div>
<div style="line-height:1.5">
<p style="color:rgb(69,75,87); font-size:12px; line-height:normal; margin-top:0px; margin-bottom:1em; font-family:Arial,Helvetica,sans-serif">
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></p>
<p style="color:rgb(69,75,87); line-height:normal; margin-top:0px; margin-bottom:1em">
<span style="font-family:Arial"><span style="font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; font-size:12px"><span style="font-family:Arial">The</span></span>&nbsp;<span style="font-size:18px">Blackfin plug-ins</span>&nbsp;<span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; font-size:12px"><span style="font-family:Arial">can
 be obtained from the</span>&nbsp;</span><a href="http://blackfin.uclinux.org/eclipse/" title="http://blackfin.uclinux.org/eclipse/">http://blackfin.uclinux.org/eclipse/</a><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; font-size:12px">&nbsp;<span style="font-family:Arial">site
 via the</span>&nbsp;</span></span><a href="http://wiki.eclipse.org/FAQ_What_is_the_Update_Manager?" title="http://wiki.eclipse.org/FAQ_What_is_the_Update_Manager?">Update Manage<span style="font-family:Arial,Helvetica,sans-serif">r</span></a><span style="font-family:Arial,Helvetica,sans-serif; line-height:1.5; font-size:12px">:</span></span></p>
</div>
</li></ol>
<span style="color:#828A9F"><span style="font-family:Arial; line-height:1.5em; font-size:12px">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 1.Select Help→Install New Software from the Eclipse menu</span><br style="line-height:1.5">
<span style="font-family:Arial; line-height:18px; font-size:12px">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span><span style="font-family:Arial; line-height:1.5em; font-size:12px">2.Click the Add button</span><br style="line-height:1.5">
<span style="font-family:Arial; line-height:18px; font-size:12px">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span><span style="font-family:Arial; font-family:Arial; font-size:12px; line-height:1.5em">3.Add&nbsp;</span><span style="font-family:Arial; line-height:1.5em; font-size:12px"><a href="http://blackfin.uclinux.org/eclipse/" target="_blank">http://blackfin.uclinux.org/eclipse/</a>&nbsp;</span><span style="font-family:Arial; line-height:1.5em; font-size:12px">as
 a new site&quot;Blackfin GNU&quot;</span></span></div>
<div>
<p style="font-size:12px; line-height:normal; font-family:Arial,Helvetica,sans-serif; margin-top:0px; margin-bottom:1em; color:rgb(69,75,87)">
<a href="http://docs.blackfin.uclinux.org/lib/exe/detail.php?id=toolchain:eclipse:install&amp;media=toolchain:eclipse:junoeclipseidebfupdatesite.png" title="toolchain:eclipse:junoeclipseidebfupdatesite.png"><img src="http://docs.blackfin.uclinux.org/lib/exe/fetch.php?media=toolchain:eclipse:junoeclipseidebfupdatesite.png" alt="" name="image_operate_79031355810938053" style="border:0px; display:block; margin:0px auto"></a></p>
<p style="font-size:12px; line-height:normal; font-family:Arial,Helvetica,sans-serif; margin-top:0px; margin-bottom:1em; color:rgb(69,75,87)">
Then you can select the plug-ins to install:</p>
<ol style="font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin:0px 0px 1em 3.5em; padding-left:0px">
</ol>
<ol style="font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin:0px 0px 1em 3.5em; padding-left:0px">
</ol>
<p style="font-size:14px; line-height:1.5; text-align:left; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
&nbsp; &nbsp; &nbsp;&nbsp;<img src="http://docs.blackfin.uclinux.org/lib/exe/fetch.php?media=toolchain:eclipse:junoeclipseidebfupdatesiteplugins.png" width="644" height="652" name="image_operate_80931355811423487" alt="" style="border:0px; max-width:690px"><br style="line-height:1.5">
</p>
<p style="font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; text-align:justify">
<span style="font-family:Arial,Helvetica,sans-serif; line-height:normal; color:rgb(69,75,87); font-size:12px; text-align:left">The Blackfin Debug plug-in results in extra selectable entries in the “Debug Configuration” dialog, while the Tool chain plug-in allows
 for selecting the desired tool chain out of a list from within the New Project Wizard dialog.</span></p>
<p style="font-size:14px; line-height:1.5; text-align:left; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
&nbsp;</p>
<h3 style="font-size:17px; line-height:normal; font-family:Arial,Helvetica,sans-serif; color:rgb(69,75,87); margin:0px 0px 1em 40px; padding:0.5em 0px 0px; border-bottom-style:none; clear:left">
<a name="zylin_cdt">Install the Zylin CDT</a></h3>
<div style="font-size:12px; line-height:normal; font-family:Arial,Helvetica,sans-serif; margin-left:43px; color:rgb(69,75,87)">
<p style="margin-top:0px; margin-bottom:1em">The Eclipse C/C&#43;&#43; Development Tools (CDT) has excellent&nbsp;<acronym title="GNU Debugger" style="border-bottom-width:1px; border-bottom-style:dotted; border-bottom-color:rgb(69,75,87)">GDB</acronym>&nbsp;support. However,
 there are a few stumbling blocks when trying to debug embedded applications, which&nbsp;<a href="http://opensource.zylin.com/embeddedcdt.html" title="http://opensource.zylin.com/embeddedcdt.html">Zylin AS</a>&nbsp;has made some modifications in Eclipse CDT to improve
 support for&nbsp;<acronym title="GNU Debugger" style="border-bottom-width:1px; border-bottom-style:dotted; border-bottom-color:rgb(69,75,87)">GDB</acronym>&nbsp;embedded debugging.</p>
<p style="margin-top:0px; margin-bottom:1em">Path for update manager “Add Site”:&nbsp;<a href="http://opensource.zylin.com/zylincdt" title="http://opensource.zylin.com/zylincdt">http://opensource.zylin.com/zylincdt</a></p>
</div>
<p style="margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; text-align:justify">
</p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:1.5; text-align:left">
<span style="line-height:1.5">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</span><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s6.sinaimg.cn/orignal/55a4cddcgd11a621b1c65" target="_blank" style="line-height:1.5"><img src="http://s6.sinaimg.cn/mw690/55a4cddcgd11a621b1c65&amp;690" name="image_operate_39801355811425546" alt="" style="border:0px"></a></div>
<div style="text-align:left"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif"><span style="font-size:14px; line-height:21px"><br>
</span></span></div>
<p></p>
<p style="text-align:left; font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
&nbsp;</p>
<h3 style="font-size:17px; line-height:normal; font-family:Arial,Helvetica,sans-serif; color:rgb(69,75,87); margin:0px 0px 1em 40px; padding:0.5em 0px 0px; border-bottom-style:none; clear:left">
<a name="subclipse_svn">Install Subclipse (SVN) &nbsp; (I skipped this)</a></h3>
<div style="font-size:12px; line-height:normal; font-family:Arial,Helvetica,sans-serif; margin-left:43px; color:rgb(69,75,87)">
<p style="margin-top:0px; margin-bottom:1em">While not released with Eclipse, there is a Subversion plugin called&nbsp;<a href="http://subclipse.tigris.org/" title="http://subclipse.tigris.org/">Subclipse</a>. Visit the Subclipse homepage for some&nbsp;<a href="http://subclipse.tigris.org/install.html" title="http://subclipse.tigris.org/install.html">concise
 install directions</a>.</p>
<p style="margin-top:0px; margin-bottom:1em">Once you have Subclipse installed, the interface is the same as using the&nbsp;<acronym title="Concurrent Versions System" style="border-bottom-width:1px; border-bottom-style:dotted; border-bottom-color:rgb(69,75,87)">CVS</acronym>&nbsp;plugin.</p>
</div>
<p style="text-align:left; font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
&nbsp;</p>
<h3 style="font-size:17px; line-height:normal; font-family:Arial,Helvetica,sans-serif; color:rgb(69,75,87); margin:0px 0px 1em 40px; padding:0.5em 0px 0px; border-bottom-style:none; clear:left">
<a name="target_management_terminal">Target Management Terminal</a></h3>
<div style="font-size:12px; line-height:normal; font-family:Arial,Helvetica,sans-serif; margin-left:43px; color:rgb(69,75,87)">
<p style="margin-top:0px; margin-bottom:1em">An ANSI (vt102) compatible Terminal including plug-ins for&nbsp;<strong>Serial</strong>,&nbsp;<strong><acronym title="Secure Shell" style="border-bottom-width:1px; border-bottom-style:dotted; border-bottom-color:rgb(69,75,87)">SSH</acronym></strong>&nbsp;and&nbsp;<strong>Telnet</strong>&nbsp;connections.</p>
<p style="margin-top:0px; margin-bottom:1em"><a href="http://docs.blackfin.uclinux.org/lib/exe/detail.php?id=toolchain:eclipse:install&amp;media=toolchain:eclipse:eclipse-target-terminal.jpg" title="toolchain:eclipse:eclipse-target-terminal.jpg"><img src="http://docs.blackfin.uclinux.org/lib/exe/fetch.php?w=800&amp;media=toolchain:eclipse:eclipse-target-terminal.jpg" alt="" width="800" name="image_operate_95001355812270067" style="border:0px; margin:0px 3px"></a></p>
<p style="margin-top:0px; margin-bottom:1em"><span style="font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px; color:rgb(0,0,0); font-size:14px">To use a serial terminal in Eclipse Juno. (Here we use Helios)</span></p>
<p style="margin-top:0px; margin-bottom:1em">&nbsp;</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">1: Install the software for serial terminals:</span></p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
Navigate to:&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Help</span>&nbsp;-&gt;&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Install New Software...</span></p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
Dropdown list for&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Work with:</span>&nbsp;to say&nbsp;<code style="margin:0px; padding:1px 5px; border:0px; vertical-align:baseline; background-color:rgb(238,238,238); font-family:Consolas,Menlo,Monaco,'Lucida Console','Liberation Mono','DejaVu sans Mono','Bitstream Vera sans Mono','Courier new',monospace,serif">Juno
 - http://download.eclipse.org/releases/juno</code></p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
Select:&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Mobile and Device Development</span>, especially&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Target
 Management Terminal</span>&nbsp;which is &quot;An ANSI (vt102) compatible Terminal including plug-ins for Serial, SSH and Telnet connections.&quot;</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
Click&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Next</span>&nbsp;and anything else to finish the install ...</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">2: Open the view</span></p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
Navigate to:&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Window</span>&nbsp;-&gt;&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Show View</span>&nbsp;-&gt;&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Other
 ...</span>&nbsp;-&gt;&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Terminal</span>&nbsp;-&gt;&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Terminal</span>&nbsp;(NOTE:
 singular&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Terminal</span>, not plural&nbsp;<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">Terminals</span>)</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
<span style="margin:0px; padding:0px; border:0px; vertical-align:baseline; background-color:transparent">3: Open a terminal</span></p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
The rest should be fairly obvious as the view contains icons to Connect, Disconnect, Settings, etc which are related to Serial, SSH and Telnet connections.</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
Reference: click&nbsp;<a href="http://stackoverflow.com/questions/12140381/how-to-open-a-serial-terminal-in-eclipse-juno" target="_blank">here</a></p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-size:14px; vertical-align:baseline; clear:both; word-wrap:break-word; color:rgb(0,0,0); font-family:Arial,'Liberation sans','DejaVu sans',sans-serif; line-height:18px">
<a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s16.sinaimg.cn/orignal/55a4cddcg7b4f77880e6f" target="_blank"><img src="http://s16.sinaimg.cn/mw690/55a4cddcg7b4f77880e6f&amp;690" width="690" height="519" name="image_operate_88141355812466450" alt="" style="border:0px"></a><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</p>
</div>
<p style="text-align:left; font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
&nbsp;</p>
<p style="font-size:12px; line-height:normal; font-family:Arial,Helvetica,sans-serif; margin-top:0px; margin-bottom:1em; color:rgb(69,75,87)">
Target Management Terminal Serial Connector requires&nbsp;<a href="http://rxtx.qbang.org/wiki/index.php/Main_Page" title="http://rxtx.qbang.org/wiki/index.php/Main_Page">RXTX</a></p>
<p style="font-size:12px; line-height:normal; font-family:Arial,Helvetica,sans-serif; margin-top:0px; margin-bottom:1em; color:rgb(69,75,87)">
Installation as an Eclipse Plugin via Update Manager:</p>
<hr style="text-align:left; font-size:12px; line-height:normal; font-family:Arial,Helvetica,sans-serif; border-right-width:0px; border-bottom-width:0px; border-left-width:0px; border-top-style:solid; border-top-color:rgb(140,172,187); height:0px; color:rgb(69,75,87)">
<ul style="font-size:12px; line-height:1.5em; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; list-style-type:square; margin:0px 0px 1em 3.5em; color:rgb(158,168,66); padding-left:0px">
<li>
<div style="line-height:1.5; color:rgb(69,75,87)"><span style="font-family:Arial">In Eclipse, choose Help &gt; Software Updates</span><span style="font-family:Arial,Helvetica,sans-serif">…</span></div>
<ul style="list-style-type:square; line-height:1.5em; margin:0px 0px 0px 1.5em; padding-left:0px">
<li>
<div style="line-height:1.5; color:rgb(69,75,87)"><span style="font-family:Arial">Add New Remote Site:</span></div>
</li></ul>
</li></ul>
<pre style="font-size:14px; line-height:normal; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; white-space:normal; margin-top:0px; padding:0.5em; border:1px dashed rgb(140,172,187); color:rgb(69,75,87); overflow:auto">Name = RXTX URL = http://rxtx.qbang.org/eclipse/</pre>
<p style="font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; text-align:justify">
<span style="font-family:Arial,Helvetica,sans-serif; line-height:18px; color:rgb(69,75,87); font-size:12px; text-align:left">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</span>&nbsp;<span style="font-family:Arial,Helvetica,sans-serif; line-height:18px; font-size:12px; text-align:left"><span style="color:#B9D478">&nbsp;</span><strong><span style="color:#6C0000">·&nbsp;</span></strong></span><span style="font-family:Arial,Helvetica,sans-serif; line-height:18px; color:rgb(69,75,87); font-size:12px; text-align:left">select
 proper version, Install All</span></p>
<p style="margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; text-align:justify">
</p>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:1.5; text-align:left">
<a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s8.sinaimg.cn/orignal/55a4cddcgd11ad0a75657" target="_blank" style="line-height:1.5"><img src="http://s8.sinaimg.cn/mw690/55a4cddcgd11ad0a75657&amp;690" width="690" height="302" name="image_operate_80341355813140964" alt="" style="border:0px"></a></div>
<div style="text-align:left"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; color:#111111"><span style="font-size:15px; line-height:22px"><br>
</span></span></div>
<span style="font-family:Tahoma,Verdana,Arial,sans-serif; color:#111111"></span>
<div style="text-align:left"><span style="font-family:Tahoma,Verdana,Arial,sans-serif; color:#111111"><span style="font-size:15px; line-height:18.233333587646484px">Untill Now, we have i</span></span><span style="font-size:15px; line-height:18.233333587646484px">nstalled
 the components:&nbsp;</span><span style="font-size:15px; line-height:18.233333587646484px; text-align:left">&nbsp;</span></div>
<p></p>
<ul style="font-size:14px; line-height:18.233333587646484px; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin:0px; padding:0px">
<li style="margin:0px 0px 0px 20px; padding:0px"><span style="line-height:1.5; color:rgb(17,17,17); font-size:15px"><span style="font-family:Arial">Blackfin ucLinux</span>&nbsp;<span style="font-family:Tahoma,Verdana,Arial,sans-serif">(</span></span><span style="font-family:Arial; line-height:normal"><span style="color:#FF0000">Blackfin
 plug-ins</span></span><span style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:1.5; color:rgb(17,17,17); font-size:15px">)</span></li><li style="font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px; color:rgb(17,17,17); margin:0px 0px 0px 20px; padding:0px">
GDB Hardware Debugging</li><li style="font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px; color:rgb(17,17,17); margin:0px 0px 0px 20px; padding:0px">
Target Terminal Management</li><li style="font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px; margin:0px 0px 0px 20px; padding:0px">
<span style="color:#111111">ZynxCDT (</span><span style="color:#FF0000">We installed ZylinCDT instead</span><span style="color:#111111">)</span></li></ul>
<div style="font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif">
<br style="line-height:1.5">
</div>
<h1 style="text-align:left; font-size:22px; line-height:1.2em; font-family:Tahoma,Verdana,Arial,sans-serif; margin:1em 0px 0.4em; padding:0px; color:rgb(17,17,17)">
Section3. Library Installation</h1>
<p style="text-align:left; font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
&nbsp;</p>
<div style="font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin:0px; padding:0px; float:left; clear:both; width:1165px">
<ul style="color:rgb(17,17,17); font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px; line-height:18.233333587646484px; margin:0px; padding:0px">
<li style="margin:0px 0px 0px 20px; padding:0px">Checkout of common code</li></ul>
<pre style="white-space:normal; color:rgb(17,17,17); font-family:'Courier new',monospace,serif; font-size:15px; line-height:19px; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); background-color:rgb(247,249,250)">&gt; svn co&nbsp;<a href="https://nuforge.coe.neu.edu/svn/pal/tags/v1.1_gnu/0.1/bare-c">https://nuforge.coe.neu.edu/svn/pal/tags/v1.1_gnu/0.1/bare-c</a></pre>
<ul style="color:rgb(17,17,17); font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px; line-height:18.233333587646484px; margin:0px; padding:0px">
<li style="margin:0px 0px 0px 20px; padding:0px">Edit Makefile.macros located in bare-c/common
<ul style="margin:0px; padding:0px">
<li style="margin:0px 0px 0px 20px; padding:0px">Edit the prefix to the path where you want the library to be installed&nbsp; For ex:&nbsp;</li></ul>
</li></ul>
<pre style="white-space:normal; color:rgb(17,17,17); font-family:'Courier new',monospace,serif; font-size:15px; line-height:19px; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); background-color:rgb(247,249,250)">&gt; # --- Installation directories prefix = /home/student/agarwal/temp/pal</pre>
<ul style="color:rgb(17,17,17); font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px; line-height:18.233333587646484px; margin:0px; padding:0px">
<li style="margin:0px 0px 0px 20px; padding:0px">Edit the COMPILER macros to the path where you have installed the compiler above<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
<span style="font-family:Arial,Helvetica,sans-serif; line-height:normal; color:rgb(69,75,87); font-size:12px">The default install path for all of our toolchains is&nbsp;</span><code style="font-size:14px; color:rgb(69,75,87); line-height:normal">/opt/uClinux/</code></li></ul>
<pre style="white-space:normal; color:rgb(17,17,17); font-family:'Courier new',monospace,serif; font-size:15px; line-height:19px; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); background-color:rgb(247,249,250)">#-- Compiler COMPILER = &nbsp;&lt;COMPILER_NSTALLATION_DIR&gt; /bfin-elf/bin/bfin-elf-gcc&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">eg. #-- Compiler COMPILER = /opt/uClinux/bfin-elf/bin/bfin-elf-gcc</pre>
<span style="font-family:Tahoma,Verdana,Arial,sans-serif; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; font-size:15px">&nbsp;<br style="line-height:1.5">
</span></span><span style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px; font-size:15px; color:rgb(17,17,17)">BackUP:<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
#-- Compiler</span><br style="line-height:1.5">
<span style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px; font-size:15px; color:rgb(17,17,17)">COMPILER = /ECEnet/Apps1/sce-ext-pkg/sw/bfin_toolchain.2011R1-BETA1/bfin-elf<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin:0px; padding:0px; float:left; clear:both; width:1165px">
<ul style="margin:0px; padding:0px">
<li style="color:rgb(17,17,17); font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px; line-height:18.233333587646484px; margin:0px 0px 0px 20px; padding:0px">
For Compiling the library we need to source the blackfin compiler path .</li></ul>
<pre style="white-space:normal; color:rgb(17,17,17); font-family:'Courier new',monospace,serif; font-size:15px; line-height:19px; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); background-color:rgb(247,249,250)">For tcsh shell&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">&gt;&nbsp;setenv PATH &lt;COMPILER_NSTALLATION_DIR&gt;/bfin-elf/bin/:$PATH&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">For bash shell&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">&gt; export PATH; PATH=&lt;COMPILER_NSTALLATION_DIR&gt;/bfin-elf/bin:$PATH</pre>
<span style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); font-size:15px">&nbsp; &nbsp; Here we use:</span></div>
<div style="font-size:14px; line-height:1.5; font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin:0px; padding:0px; float:left; clear:both; width:1165px">
<pre style="white-space:normal; color:rgb(17,17,17); font-size:15px; line-height:19px; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); background-color:rgb(247,249,250)"><span style="font-family:'Courier new',monospace,serif; line-height:1.5">&gt; export PATH; PATH = /</span>opt/uClinux/bfin-elf/bin:$PATH&nbsp;</pre>
</div>
<div style="font-size:14px; line-height:1.5; margin:0px; padding:0px; float:left; clear:both; width:1165px">
<ul style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; margin:0px; padding:0px">
<li style="color:rgb(17,17,17); font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px; line-height:18.233333587646484px; margin:0px 0px 0px 20px; padding:0px">
Follow these steps to install the libraries</li></ul>
<pre style="font-family:'Courier new',monospace,serif; white-space:normal; color:rgb(17,17,17); font-size:15px; line-height:19px; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); background-color:rgb(247,249,250)">&gt; cd common &nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">&gt; make clean&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">&gt; make&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">&gt; sudo make install</pre>
<ul style="font-family:Tahoma,Verdana,Arial,sans-serif; color:rgb(17,17,17); font-size:15px; line-height:18.233333587646484px; margin:0px; padding:0px">
<li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; Validate your environment&nbsp;&nbsp;&nbsp;</li></ul>
<pre style="font-family:'Courier new',monospace,serif; white-space:normal; color:rgb(17,17,17); font-size:15px; line-height:19px; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); background-color:rgb(247,249,250)">&gt; source &lt;installation path&gt;/bin/setup.sh&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5"></pre>
<pre style="font-family:'Courier new',monospace,serif; white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187)">&nbsp;eg. &gt; source /usr/uClinux/bfin-elf/bin/setup.sh</pre>
<pre style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; white-space:normal">&gt; cd demo&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">&gt; make clean&nbsp;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">&gt; make</pre>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; font-size:15px; margin:0px; padding:0px">
<span style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:1.5; color:rgb(17,17,17)">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; This make should complete without error. (</span><span style="font-family:Arial; color:#FF0000">But we meet several errors</span><span style="font-family:Tahoma,Verdana,Arial,sans-serif; color:rgb(17,17,17)">)</span></div>
<div style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); font-size:15px; margin:0px; padding:0px">
&nbsp; ==============================================</div>
<div style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); font-size:15px; margin:0px; padding:0px">
Here, I just referred some other instructions, and:</div>
<div style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); font-size:15px; margin:0px; padding:0px">
&nbsp;1.deleted Eclipse,&nbsp;</div>
<div style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); font-size:15px; margin:0px; padding:0px">
&nbsp;2.Downloaded the Eclipse(Juno)</div>
<div style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); font-size:15px; margin:0px; padding:0px">
&nbsp;3.Set the PATH</div>
<div style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); font-size:15px; margin:0px; padding:0px">
<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</div>
<div style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-size:20px">1. ReInstall the Eclipse</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; go to the &quot;eclipse&quot; folder:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&gt; cd /opt</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&gt; rm -rf eclipse</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Verdana; font-size:20px">2. Download the Eclipse (Juno)</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><u><em><strong><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</strong></em></u></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
&nbsp; &nbsp;&nbsp;<span style="font-family:Arial">Select the Juno version,&nbsp;</span><span style="font-family:Arial">Download the eclipse package from:&nbsp;</span><a href="http://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/juno/SR1/eclipse-cpp-juno-SR1-linux-gtk.tar.gz" target="_blank">Linux
 32bit</a></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp;</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp;Extra it into /home/yourname/programfiles/</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><u><em><strong><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</strong></em></u></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp;We have Installed the Java Runtime Environment above, so we don't need to install it here.</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-size:20px"><span style="font-family:Verdana; color:rgb(17,17,17); line-height:18.233333587646484px">3. Configure the</span>&nbsp;<span style="font-family:Verdana; line-height:18.233333587646484px; color:rgb(17,17,17)">environment variable(PATH)</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Verdana; color:rgb(17,17,17); line-height:18.233333587646484px"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Verdana; color:rgb(17,17,17); line-height:18.233333587646484px">&nbsp; &nbsp; &nbsp; &nbsp;Modify the environment variable of &quot;bfin-linux-uclibc-gcc&quot;</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:rgb(17,17,17); line-height:18.233333587646484px">&nbsp; &nbsp; &nbsp; &nbsp; In Terminal, run:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<pre style="white-space:normal; font-family:'Courier new',monospace,serif; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187)"><span style="font-family:'Courier new'; line-height:18.233333587646484px; color:rgb(17,17,17)">&gt; sudo gedit /etc/profile</span></pre>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="color:rgb(17,17,17); line-height:18.233333587646484px"><span style="font-family:Arial">&nbsp;</span></span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp;&nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">Modify
 the Environmental Variable. Note, this is user environmental variable, After this we can only use</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px"><span style="font-family:Arial">&quot;bfin-linux-uclibc-gcc&quot; but cannot use &quot;sudo&nbsp;</span></span></span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">bfin-linux-uclibc-gcc</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&quot;.
 All the command after &quot;sudo&quot; is for using&nbsp;</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">system environmental variable but not user's environmental variable.</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;In the gedit window add the line below at the tail.</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<pre style="white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187)"><span style="font-family:'Courier new'; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;&gt; export PATH=$PATH:/opt/uClinux/bfin-linux-uclibc/bin</span><span style="font-family:'Courier new'; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span></pre>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="line-height:1.5; color:rgb(17,17,17)">&nbsp; &nbsp;&nbsp;<span style="font-family:Arial">And for the JRE PATH configure, add the line below:</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<pre style="white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187)"><span style="font-family:'Courier new'; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;&gt; JAVA_HOME=/opt/java/32/jre1.6.0_16<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">&nbsp;&gt; PATH=$JAVA_HOME/bin:$PATH</span></pre>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp;See the snap below:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="color:#111111">&nbsp; &nbsp;&nbsp;</span><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s8.sinaimg.cn/orignal/55a4cddcgd12acb47ed37" target="_blank"><img src="http://s8.sinaimg.cn/mw690/55a4cddcgd12acb47ed37&amp;690" name="image_operate_79891355882300468" alt="" style="border:0px"></a></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Verdana; font-size:20px">4. Building a uClinux Hello World project&nbsp;</span><span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Go to the location of eclipse:&quot;</span><span style="font-family:Arial">/home/yourname/programfiles/eclipse</span><span style="font-family:Arial">&quot;, Run the eclipse by:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<pre style="white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187)"><span style="font-family:'Courier new'; line-height:18.233333587646484px; color:rgb(17,17,17)">&gt; cd&nbsp;</span><span style="color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px"><span style="font-family:Courier New">/home/yourname/programfiles/eclipse</span><br style="line-height:1.5"></span></span><span style="font-family:'Courier new'; line-height:18.233333587646484px; color:rgb(17,17,17)">&gt; ./eclipse</span><span style="font-family:'Courier new'; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span></pre>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; In Eclipse, New-&gt;Project-&gt;C/C&#43;&#43; -&gt; C Project -&gt; &quot;Project type: Executable/Empty Project&quot; and&nbsp;</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&quot;ToolChains:Cross GCC &quot; -&gt; &quot;Next&quot; -&gt; Set the path of your &quot;bfin-linux-uclibc-gcc&quot; &nbsp;-&gt; finish.</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">See the snap:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
&nbsp; &nbsp; &nbsp; &nbsp;<a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s16.sinaimg.cn/orignal/55a4cddcgd12af44ee9ef" target="_blank"><img src="http://s16.sinaimg.cn/bmiddle/55a4cddcgd12af44ee9ef&amp;690" name="image_operate_6471355882674426" alt="" style="border:0px"></a><br style="line-height:1.5">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
&nbsp; &nbsp; &nbsp; &nbsp;<span style="font-family:Arial">And build your Hello World project:</span><br style="line-height:1.5">
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<div style="line-height:1.5">&nbsp; &nbsp; &nbsp; &nbsp;<a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s10.sinaimg.cn/orignal/55a4cddcgd12b02c1a3a9" target="_blank"><img src="http://s10.sinaimg.cn/bmiddle/55a4cddcgd12b02c1a3a9&amp;690" name="image_operate_39601355883012759" alt="" style="border:0px"></a></div>
<br style="line-height:1.5">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; This method is only for building the uClinux app. If you want to build Bare-Metal&nbsp;</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="line-height:18.233333587646484px"><span style="font-family:Arial">Refer to:</span></span><span style="font-family:Arial; font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px"><br style="line-height:1.5">
</span></span><span style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:1.2em; font-size:16px">Developing Bare-Metal C Applications</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<ol style="margin:0px; padding:0px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
<li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; Setup common bare-c library&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=bareC">bareC</a>&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=bareCReleaseNotes">Release
 Notes</a>
<ol style="margin:0px; padding:0px">
<li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; Update bare-c libraries&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=updateBarec">updateBarec</a></li><li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; Doxygen documentation&nbsp;<a href="http://www.ece.neu.edu/~esl/EECE4534/common/">link</a></li></ol>
</li><li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; Developing and debugging C applications using Eclipse&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=eclipseDev">link</a>
<ol style="margin:0px; padding:0px">
<li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; &nbsp;TLL's: Bare-C Development/Debugging with Eclipse<a href="https://tll.assembla.com/spaces/designcenter/wiki/Developing_C_Applications_in_Baremetal">&nbsp;link</a><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="margin:0px; padding:0px">&nbsp; &nbsp; &nbsp;More detailed debug tutorial.</span></li><li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; &nbsp;Debug connection through gnICE&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?&amp;pageid=316#gdbProxy">link</a></li><li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; &nbsp;Alternative debug connection: using kgdb&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=kgdb">kgdb</a></li></ol>
</li><li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; &nbsp;Using DSP-specific libraries&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=dsplibs">dsplibs</a></li><li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; &nbsp;Launching bare-c apps through U-Boot&nbsp;<a href="https://tll.assembla.com/spaces/designcenter/wiki/Transferring_Files_and_Launching_Bare_C_Apps">link</a></li><li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; &nbsp;Analog Devices introduction to bare-c development&nbsp;<a href="http://docs.blackfin.uclinux.org/doku.php?id=toolchain:bare_metal">link</a></li><li style="margin:0px 0px 0px 20px; padding:0px">&nbsp; &nbsp; &nbsp;Stop automatically booting linux&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=u-boot">u-boot</a></li></ol>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><u><em><strong><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</strong></em></u></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><em><strong><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</strong></em></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">================= Connect with Target Board =================</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; In this Section, we are going to download the executable bin file made by Eclipse down to target.</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; 1. Install the minicom</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; 2. Install the USB-UART bridge converter driver on Linux</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; 3. Install the USB-Ethernet bridge converter driver on Linux, and Configure it.</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; 4. Test the Communication functions.</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; 5. Test the J-TAG connection if I have interests.</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp;&nbsp;&nbsp;<span style="font-size:20px">1. Install the minicom</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;In the Terminal, input:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; margin:0px; padding:0px; color:rgb(17,17,17)">
<pre style="white-space:normal; color:rgb(0,0,0); line-height:21px; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187)"><span style="font-family:'Courier new'; line-height:18.233333587646484px; color:rgb(17,17,17)">&gt; sudo apt-get install minicom</span></pre>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial; line-height:26px; color:rgb(0,0,0)">&nbsp; &nbsp;&nbsp;</span><span style="font-family:Arial"><span style="font-size:20px">2. Install the USB-UART bridge converter driver on Linux</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><span style="font-size:20px"><span style="font-family:Verdana; font-size:14px; line-height:21px">&nbsp; &nbsp; &nbsp; &nbsp; Install driver for USB-UART bridge converter on Linux Ubuntu12.04</span><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; font-size:14px; line-height:21px"></span></span></span>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<span style="color:rgb(17,17,17); font-family:Verdana; font-size:14px; line-height:21px">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>Ubuntu下USB转串口芯片驱动程序安装，支持cp210x,pl2303等<br style="line-height:1.5">
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><span style="color:rgb(17,17,17); font-family:Verdana; font-size:14px; line-height:21px">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>Reference:&nbsp;<a href="http://pharos.ece.utexas.edu/wiki/index.php/Fixing_the_cp210x_open_-_Unable_to_enable_UART_Error_-_04/17/2011" target="_blank">Fixing
 the cp210x open - Unable to enable UART Error</a></span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><span style="color:rgb(17,17,17); font-family:Verdana; font-size:14px; line-height:21px">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span>When you plugin your USB-UART converter, and run &quot;</span><span style="font-family:Courier New"><strong>&gt;
 ls /dev/tty*</strong></span><span style="font-family:Verdana">&quot;, if you don't see the<br>
&nbsp;/dev/ttyUSB0 (or similar), your Linux does not detect your USB-UART device.</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana">We need to install the driver for your device.</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana">Here we use Ubuntu12.04, and Updated the source to 3.2.0 version. If there is difference about<br>
version Number from your OS platform, please try to modify it into yours.</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5">
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana; font-size:18px">1.Download the Linux Source Code</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana">Open a terminal and execute the following commands. Note that your version of Linux may differ<br>
slightly -- adjust accordingly.</span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ cd ~</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ sudo apt-get install build-essential linux-source</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ cp /usr/src/linux-source-3.2.0.tar.bz2 .</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ bunzip2 linux-source-3.2.0.tar.bz2&nbsp;</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ tar xf linux-source-3.2.0.tar&nbsp;</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ cd ~/linux-source-3.2.0</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana; font-size:18px">2.Recompile and Reinstall the cp210x Driver</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana">From within a terminal, execute:</span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ cd ~/linux-source-3.2.0</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ make oldconfig</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ make prepare</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ make scripts</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ cp /usr/src/linux-headers-3.2.0-34-generic-pae/Module.symvers .</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Verdana">Here, I have the &quot;3.2.0</span><span style="font-family:Verdana; line-height:1.5">-29&quot; version also, I launched the command above, but not the below:</span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>&nbsp; &quot;cp /usr/src/linux-headers-3.2.0-29-generic-pae/Module.symvers .&quot;</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5">
<div style="line-height:1.5"><span style="font-family:Verdana">Recompile and Reinstall the cp210x Driver</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana">Here, We can actually install many kinds of USB-UART converter drivers. We take cp210x as the<br>
&nbsp;example.</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana">From within a terminal, execute:</span></div>
</div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ make M=drivers/usb/serial</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ sudo mv /lib/modules/$(uname -r)/kernel/drivers/usb/serial/cp210x.ko /lib/modules<br>
&nbsp; /$(uname -r)/kernel/drivers/usb/serial/cp210x.ko.old</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ sudo cp drivers/usb/serial/cp210x.ko /lib/modules/$(uname -r)/kernel/drivers/usb<br>
&nbsp; /serial/</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ sudo modprobe -r cp210x</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ sudo modprobe cp210x</strong></span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana">Reboot Linux system.</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5"><span style="font-family:Verdana">Run Terminal:&nbsp;</span></div>
<div style="line-height:1.5"><span style="font-family:Courier New"><strong>$ ls /dev/tty*</strong></span></div>
</div>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<span style="font-family:Verdana">The we can see the device is detected by Linux Host OS:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<div style="text-align:left; line-height:1.5"><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s1.sinaimg.cn/orignal/55a4cddcgd12f287c93b0" target="_blank"><img src="http://s1.sinaimg.cn/bmiddle/55a4cddcgd12f287c93b0&amp;690" title="" name="image_operate_29861355900659480" alt="" style="border:0px"></a></div>
<div style="text-align:left; line-height:1.5"><br style="line-height:1.5">
</div>
<div style="line-height:1.5"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp;Reference:&nbsp;<a href="http://pharos.ece.utexas.edu/wiki/index.php/Fixing_the_cp210x_open_-_Unable_to_enable_UART_Error_-_04/17/2011" target="_blank">http://pharos.ece.utexas.edu/wiki/index.php/Fixing_the_cp210x_open_-_Unable_to_enable_UART_Error_-_04/17/2011</a></span></div>
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<span style="font-family:Arial">&nbsp; &nbsp; Then, We can configure the minicom to communicate with our target board.</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<span style="font-family:Verdana"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<span style="font-family:Verdana">======================================================</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<span style="font-family:Verdana">Here is an example of configure the parameters of minicom for TLL6527M PAL board:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<span style="font-family:Verdana">I just copied it here hardly without any font editing. Sorry about that.</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:21px; font-size:14px">
<h3 style="padding:0px; line-height:18px; margin:21px 0px 2px!important; font-size:16px!important">
<span style="line-height:1.5; margin:0px; padding:0px; font-size:12px"><span style="font-family:Arial">Serial Communications from Host-PC to TLL6527M Target Hardware</span></span></h3>
<p style="margin-top:0px; margin-bottom:0.75em; padding-top:0px; padding-bottom:0px; font-size:13px; line-height:17px">
<span style="font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">The TLL System Design Environment (SDE) running on the host PC comes pre-configured with<br>
&nbsp;the required settings for serial-UART communications between the host PC and the TLL6527M<br>
&nbsp;base module. Generally, for use with TLL6527M the settings need not to be changed<br>
(</span><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">Note: From the SDE OS version 0.3.2, the serial terminal program Minicom is set to<br>
open /dev/ttyUSB0 with the needed configuration as mentioned above.</span><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">).</span></span></p>
<ul style="margin:6px 0px 6px 18px; padding:0px; font-size:13px; line-height:17px">
<li style="margin:0px; padding:0px"><span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">To start serial communication from host to the TLL6527M, open the serial communication terminal<br>
program Minicom. Just open a terminal window on your Linux SDE running on the host PC&nbsp; and<br>
type the command &quot;minicom&quot; at the command prompt on your host PC terminal console.</span></span></li><li style="margin:0px; padding:0px">
<p style="margin-top:0px; margin-bottom:0.75em; padding-top:0px; padding-bottom:0px">
<span style="font-family:Arial">The TLL6527M on power-up&nbsp; is set by default to the following serial-UART communication settings:</span></p>
</li></ul>
<div style="line-height:17px; font-size:13px; margin:6px 0px 6px 2em; padding:0px">
<ul style="margin:0px 0px 0px 18px; padding:0px">
<li style="margin:0px; padding:0px"><span style="font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">Bits per second = 115200</span></span></li><li style="margin:0px; padding:0px"><span style="font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">Data bits = 8</span></span></li><li style="margin:0px; padding:0px"><span style="font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">Parity = None</span></span></li><li style="margin:0px; padding:0px"><span style="font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">Stop Bits = 1</span></span></li><li style="margin:0px; padding:0px"><span style="font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">Flow control = None</span></span></li></ul>
</div>
<ul style="margin:6px 0px 6px 18px; padding:0px; font-size:13px; line-height:17px">
<li style="margin:0px; padding:0px"><span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp;Make sure that the TLL6527M's USB-UART port has been mapped to a ttyUSBx device node in the /dev<br>
folder on the host PC SDE. See following example:</span></span></li></ul>
<p style="margin-top:0px; margin-bottom:0.75em; padding-top:0px; padding-bottom:0px; font-size:13px; line-height:17px">
<br>
</p>
<p style="margin-top:0px; margin-bottom:0.75em; padding-top:0px; padding-bottom:0px; font-size:13px; line-height:17px">
<img src="http://www.assembla.com//spaces/designcenter/documents/aJOoa-yr8r4yA7acwqjQWU/download/dc9HNAyq4r4zGxacwqjQXA" border="0" name="image_operate_70081355901479967" alt="" style="text-align:left; border:0px; margin:0px auto; padding:0px; width:auto; max-width:700px; height:auto; display:block"></p>
<ul style="margin:6px 0px 6px 18px; padding:0px; font-size:13px; line-height:17px">
<li style="margin:0px; padding:0px"><span style="font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">Now make sure that this &quot;ttyUSBx&quot; is set as the destination port, baud rate is 115200,<br>
&nbsp;and 8N1 mode for minicom, by running the command 'minicom -s'. Note that running the<br>
&quot;minicom -s&quot; command as a normal user will only apply the settings for the current<br>
session. In order to make the settings permanent, these commands need to be run with<br>
root privileges.</span><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">See example below:&nbsp;</span></span></li></ul>
<p style="margin-top:0px; margin-bottom:0.75em; padding-top:0px; padding-bottom:0px; font-size:13px; line-height:17px">
<span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial"><img src="http://www.assembla.com//spaces/designcenter/documents/d3oKJCyr4r4BpAacwqjQXA/download/dc9HNAyq4r4zGxacwqjQXA" border="0" name="image_operate_8651355901479842" alt="" style="border:0px; margin:0px auto; padding:0px; width:auto; max-width:700px; height:auto; display:block"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
</span></span></p>
<ul style="margin:6px 0px 6px 18px; padding:0px; font-size:13px; line-height:17px">
<li style="margin:0px; padding:0px"><span style="font-family:Arial">The following window will appear on your host PC terminal window:&nbsp;</span></li></ul>
<p style="margin-top:0px; margin-bottom:0.75em; padding-top:0px; padding-bottom:0px; font-size:13px; line-height:17px">
<img src="http://www.assembla.com//spaces/designcenter/documents/b9mNFEyr8r4y9AacwqjQYw/download/dc9HNAyq4r4zGxacwqjQXA" border="0" alt="" style="border:0px; margin:0px auto; padding:0px; width:auto; max-width:700px; height:auto; display:block"></p>
<ul style="margin:6px 0px 6px 18px; padding:0px; font-size:13px; line-height:17px">
<li style="margin:0px; padding:0px"><span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">Go to ‘Serial port setup’, following screen will appear, make sure that serial device is set to ‘/dev/ttyUSB0’<br>
and bps/par/bits is set to 115200 8N1:</span></span></li></ul>
<div style="line-height:17px; padding:0px; font-size:13px; margin:6px 0px!important">
<span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial"><img src="http://www.assembla.com//spaces/designcenter/documents/cKH9d0yr8r4BpAacwqjQXA/download/dc9HNAyq4r4zGxacwqjQXA" border="0" name="image_operate_85671355901456846" alt="" style="border:0px; margin:0px auto; padding:0px; width:auto; max-width:700px; height:auto; display:block">&nbsp;</span></span></div>
<div style="line-height:17px; padding:0px; font-size:13px; margin:6px 0px!important">
<ul style="margin:6px 0px 6px 18px; padding:0px">
<li style="margin:0px; padding:0px"><span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">Now go back to the main menu and choose ‘save setup as dfl’. This will save the configuration settings and from next time<br>
onwards user can directly run ‘minicom’ command without the option ‘–s’ and it will load settings from the saved file.</span></span></li></ul>
</div>
<p style="margin-top:0px; margin-bottom:0.75em; padding-top:0px; padding-bottom:0px; font-size:13px; line-height:17px">
<span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial"><img src="http://www.assembla.com//spaces/designcenter/documents/dSlBjCyr8r4yA7acwqjQWU/download/dc9HNAyq4r4zGxacwqjQXA" border="0" name="image_operate_97841355901479274" alt="" style="border:0px; margin:0px auto; padding:0px; width:auto; max-width:700px; height:auto; display:block"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
</span></span></p>
<ul style="margin:6px 0px 6px 18px; padding:0px; font-size:13px; line-height:17px">
<li style="margin:0px; padding:0px"><span style="line-height:1.5; margin:0px; padding:0px; font-size:10pt"><span style="font-family:Arial">RESET the target by using the SDE's TLL6527M reset utility or by pressing the RESET button on the target.&nbsp;</span></span></li></ul>
<ul style="margin:6px 0px 6px 18px; padding:0px; font-size:13px; line-height:17px">
<li style="margin:0px; padding:0px"><span style="font-family:Verdana"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px; font-size:10pt">Following messages will be displayed on the terminal. By default,
 U-Boot is setup to <br>
automatically start booting the OS after waiting for a few seconds for the user to<br>
interrupt the automatic launch by pressing any key. So to stop U-Boot from loading<br>
uClinux and provide U-Boot command prompt, just hit any key. (This is assuming TLL6527M<br>
is flashed with u-boot and uClinux. TLL6527Ms are pre-flashed with firmware before shipping)</span>.</span></li></ul>
</div>
<br>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><span style="font-size:20px"><br>
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><span style="font-size:20px"><br>
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><span style="font-size:20px"><br>
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><span style="font-size:20px"><br>
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial; line-height:26px; color:rgb(0,0,0)">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Go to the website of your converter applier, for me:&nbsp;</span><a href="http://www.silabs.com/products/mcu/Pages/USBtoUARTBridgeVCPDrivers.aspx" target="_blank">Here</a><span style="font-family:Arial; line-height:26px; color:rgb(0,0,0)">,
 Download&nbsp;</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial; line-height:26px; color:rgb(0,0,0)">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;I halted here. :( But I never give up, solved it next:)</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; line-height:26px">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Here is another simpler way to install the driver:&nbsp;<br>
</span><span style="font-family:Arial">&nbsp;<a href="http://blog.sina.com.cn/s/blog_55a4cddc0101afs8.html" target="_blank">http://blog.sina.com.cn/s/blog_55a4cddc0101afs8.html</a></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="line-height:26px; color:rgb(0,0,0)"><span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="line-height:26px; color:rgb(0,0,0)"><span style="font-family:Arial">&nbsp; &nbsp;</span>&nbsp;<span style="font-family:Verdana; font-size:20px">3. Minicom configuration:</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:26px">&nbsp; &nbsp; &nbsp;<span style="font-family:Arial">See:&nbsp;</span></span></span><a href="http://blog.sina.com.cn/s/blog_55a4cddc0101afs8.html" target="_blank"><span style="font-family:Arial">http://blog.sina.com.cn/s/blog_55a4cddc0101afs8.html</span></a></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
&nbsp; &nbsp;<span style="font-family:Arial">To here, we can use Eclipse to develop our own Blackfin-uClinux app and send it down to our target</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; board (TLL6527M PAL Board), But there is still no our desired ToolChain&nbsp;</span><span style="font-family:Arial,Helvetica,sans-serif; line-height:normal; color:rgb(69,75,87); font-size:12px">out of a list from within the
 New Project Wizard dialog</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:1.5">&nbsp;</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial; line-height:18.233333587646484px">===========</span>&nbsp;<span style="font-family:Arial">Add the</span>&nbsp;<span style="font-family:Arial">Blackfin Linux FDPIC GNU Toolchain(bfin-linux-uclibc)&nbsp;</span><span style="font-family:Arial; line-height:1.5">=============</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<div style="line-height:1.5; margin:0px; padding:0px">
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">Then
 I installed the Blackfin Plug-ins again for Juno</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; color:rgb(17,17,17)">Reference
 :&nbsp;</span><a href="http://docs.blackfin.uclinux.org/doku.php?id=toolchain:eclipse:install" target="_blank">Installing Eclipse 4.2 (Juno)</a></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">There
 contains:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-size:12px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;
 &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:1.5">`. A Java Runtime Environment (JRE) version 1.6 or newer</span></span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-size:12px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;
 &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:1.5">`. Eclipse IDDE (version 4.2 recommended)</span></span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-size:12px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;
 &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:1.5">`. C/C&#43;&#43; Development Tools (CDT)</span></span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-size:12px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;
 &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:1.5">`. Blackfin toolchain</span></span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-size:12px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;
 &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:1.5">`. Blackfin Plug-ins</span></span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;Here we only installed
 the Plug-ins:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px">
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; color:#111111">The
 Blackfin plug-ins will require Eclipse 4.2, as well as the CDT.&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; line-height:1.5; color:rgb(17,17,17)">Additionally,
 you will have to install the GDB Hardware Debugging&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; color:#111111">CDT
 plug-in (it is usually not installed by default). To install&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial; color:#111111">the
 GDB Hardware Debugging plug-in the CDT update site needs enabled.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;--------</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:1.5; color:rgb(17,17,17)">To
 enable the CDT site:</span></div>
</div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">1.
 Select the Eclipse menu Window→Preferences.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">2.
 Select Install/Update→Available Software Sites and select the site,&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">3.
 http://download.eclipse.org/tools/cdt/releases/juno.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">4.
 Use the Enable button to enable the site.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">---------</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">Install
 the GDB Hardware Debugging plug-in via the Update Manager:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">1.
 Select the Eclipse menu Help→Install New Software</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">2.
 Select the http://download.eclipse.org/tools/cdt/releases/indigo site in the Work With list</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">3.
 Enable the GDB Hardware Debugging plugin under CDT Optional Features</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><br style="line-height:1.5">
</div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">---------</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">The
 Blackfin plug-ins can be obtained from the&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial"><u><span style="color:#874F85">http://blackfin.uclinux.org/eclipse/</span></u>&nbsp;&nbsp;
 &nbsp;</span><span style="font-family:Arial; line-height:1.5">site via the Update Manager:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">1.
 Select Help→Install New Software from the Eclipse menu</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">2.
 Click the Add button</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">3.
 Add http://blackfin.uclinux.org/eclipse/ as a new site &quot;Blackfin GNU&quot;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">---------</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">Then
 you can select the plug-ins to install:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">The&nbsp;<strong>Blackfin
 Debug plug-in</strong>&nbsp;results in extra selectable entries in the</span><span style="font-family:Arial; line-height:1.5">&nbsp;“Debug Configuration” dialog,&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;T</span><span style="font-family:Arial">he&nbsp;<strong>Tool
 chain plug-in</strong>&nbsp;allows</span>&nbsp;<span style="font-family:Arial; line-height:1.5">for selecting the desired tool chain out of a list from within the&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">New
 Project Wizard dialog</span>.</div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">As
 the dialoge box said, restart Eclipse.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span>---------</div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">Then
 when we creat a C Project, &quot;Project type&quot;:Empty Project, in the &quot;ToolChains&quot; box,&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp;</span><span style="font-family:Arial">there
 appears:&quot;Blackfin Linux FDPIC GNU Toolchain(bfin-linux-uclibc)</span>&quot;</div>
</div>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial">&nbsp;</span><span style="font-family:Arial">See the Snap pic below:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s5.sinaimg.cn/orignal/55a4cddcgd1396bb87584" target="_blank"><img src="http://s5.sinaimg.cn/bmiddle/55a4cddcgd1396bb87584&amp;690" name="image_operate_14171355944472128" alt="" style="border:0px"></a></div>
<br style="line-height:1.5">
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">======================================</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<div style="line-height:1.5; margin:0px; padding:0px"><span style="color:#111111">=========== Ethernet Connection ==========</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="color:#111111"><br style="line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span style="font-family:Arial">Here, I'm going to configure the Ethernet manually.</span></span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; I tried to run palTelnet.sh, but failed by some errors.:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">-----------</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">./palTelnet.sh: 44: read: arg count</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">WARNING: TRENDnet TU2-ET100 not found.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp; 1) Validate USB connection from TU2-ET100 to host.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp; 2) Validate TU2-ET100 is connected to virtual machine instead of host).</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp;check: Virtual Machine -&gt; Removable Devices -&gt; asix ax88772</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">Press &lt;ENTER&gt; to continue, &lt;CTRL&gt;-C to stop</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">./palTelnet.sh: 44: read: arg count</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">-----------</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; It says the TRENDnet TU2-ET100, I thought it was because of the driver</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; of TU2-ET100 was not configured.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; (The gray part does not work, you can just skip it)</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;</span>&nbsp;<span style="font-family:Arial; color:#A4A4B2">Download the driver from:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; http://www.trendnet.com/downloads/list_subcategory.asp?SUBTYPE_ID=1003</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; Extract it into:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; /home/sonictl/Downloads/LINUX2.6.9_REV122/AX88772_772A_LINUX2.6.9_REV122/</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">open readme, it says:&quot;This driver is only for Kernel 2.6.9 to 2.6.13&quot;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">It seems like I need to download the Kernel2.6.9. get it&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; from:get &quot;linux-2.6.9.tar.bz2&quot; from: www.kernel.org/pub/linux/kernel/v2.6/</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; Open a terminal and execute the following commands.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cd ~</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ sudo mv ~/Downloads/linux-2.6.9.tar.bz2 /usr/src/</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cp /usr/src/linux-2.6.9.tar.bz2 .</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ bunzip2 linux-2.6.9.tar.bz2</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ tar xf linux-2.6.9.tar&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cd ~/linux-2.6.9</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; Recompile and Reinstall the cp210x Driver,From within a terminal, execute:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cd ~/linux-2.6.9</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ make oldconfig</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">I pressed all Return Key.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ make prepare</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ make scripts</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#A4A4B2">&nbsp; &nbsp; &nbsp; &nbsp; &gt; $ cp /usr/src/linux-headers-3.2.0-34-generic-pae/Module.symvers .</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">To here I get halted. I searched online for a while, and Did the following:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="color:#111111">========================================================</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="color:#111111"><br style="line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span style="font-family:Arial">Retry Install the ASIX AX88772 Driver for Linux</span></span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="color:rgb(17,17,17)">&nbsp;</span><span style="color:#111111"><span style="font-family:Arial">Download the driver for Kernel 3.x</span>&nbsp;<span style="font-family:Impact">（</span><span style="font-family:Arial">my
 kernel is&nbsp;3.2.0, you can update your kernel by&nbsp;</span></span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="color:#111111"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &gt;&nbsp;</span></span><span style="color:rgb(70,70,70); font-family:'Courier new'; background-color:rgb(188,211,229)">sudo apt-get install
 build-essential linux-source</span><span style="line-height:1.5; color:rgb(17,17,17)"><span style="font-family:Impact">&nbsp; &nbsp;&nbsp;</span><span style="font-family:宋体">)</span></span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp;</span><span style="color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; color:#111111">&nbsp;unzip the Package_YouDownloaded.zip</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial; color:#111111">&nbsp;</span><span style="color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span><span style="font-family:Arial; color:#111111">go into the folder, and open the readme file in gedit</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><a href="http://blog.photo.sina.com.cn/showpic.html#url=http://s15.sinaimg.cn/orignal/55a4cddcgd140587f2ede" target="_blank"><img src="http://s15.sinaimg.cn/mw690/55a4cddcgd140587f2ede&amp;690" name="image_operate_32601355974621425" alt="" style="border:0px"></a></div>
<pre style="white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); background-color:rgb(247,249,250)"><div style="text-align:left"><span style="background-color:rgb(255,255,255); font-family:'Courier new'; text-align:justify; white-space:pre-wrap; line-height:1.5">$ sudo su</span></div><span style="font-family:'Courier new'; white-space:pre-wrap; background-color:rgb(255,255,255)">#&nbsp;</span><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; background-color:rgb(255,252,246)"><span style="font-family:Courier New">make</span></span></pre>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span><span style="color:#111111"><span style="font-family:Arial; font-family:Arial">If the compilation is well, the</span>&nbsp;<span style="font-family:Courier New"><strong>asix.ko</strong></span>&nbsp;<span style="font-family:Arial; font-family:Arial">will
 be created under the current</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="color:#111111"><span style="line-height:18.233333587646484px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; directory.<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="color:#111111"><span style="line-height:18.233333587646484px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></span></span><span style="color:#111111"><span style="line-height:18.233333587646484px"><span style="font-family:Arial; font-family:Arial">If
 you want to use modprobe command to mount the driver, executing th</span><span style="font-family:Arial">e</span></span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px">&nbsp; &nbsp;&nbsp;<span style="font-family:Arial">following command to install the driver into your Linux:</span></span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<pre style="text-align:left; white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); background-color:rgb(247,249,250)"><span style="font-family:'Courier new'; white-space:pre-wrap; text-align:justify; background-color:rgb(255,255,255)">#&nbsp;</span><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17)"><span style="font-family:Courier New">[root@localhost template]# make install</span></span></pre>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px">&nbsp; &nbsp; &nbsp;----------------</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px">&nbsp; &nbsp; &nbsp;<span style="font-family:Arial">Usage:</span></span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px">&nbsp; &nbsp; &nbsp;----------------</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px"><br style="line-height:1.5">
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px">1.&nbsp;<span style="font-family:Arial">If
 you want to load the driver manually, go to the driver directory and</span></span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px">&nbsp; &nbsp;</span></span><span style="line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">execute
 the following commands: (</span>&nbsp;<span style="font-family:Arial; line-height:18.233333587646484px"><span style="color:#FF0000">this does not work, skip</span></span>&nbsp;<span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">)</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px"><br style="line-height:1.5">
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="line-height:18.233333587646484px"><span style="font-family:Courier New; color:#2110FF">[root@localhost template]# insmod asix.ko</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px"><br style="line-height:1.5">
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px">2.&nbsp;<span style="font-family:Arial">If
 you had installed the driver during driver compilation, then you can use</span></span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px">&nbsp; &nbsp;</span></span><span style="line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">the
 following command to load the driver automatically. (</span><span style="font-family:Arial; line-height:18.233333587646484px"><span style="color:#FF0000">I used this one</span></span><span style="font-family:Arial; line-height:18.233333587646484px; color:rgb(17,17,17)">)</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px"><br style="line-height:1.5">
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="line-height:18.233333587646484px"><span style="font-family:Courier New; color:#391DFB">[root@localhost anywhere]# modprobe asix</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px"><br style="line-height:1.5">
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="line-height:18.233333587646484px; color:rgb(17,17,17)">&nbsp; &nbsp; &nbsp;</span><span style="color:#111111"><span style="line-height:18.233333587646484px"><span style="font-family:Arial; font-family:Arial">If</span>&nbsp;<span style="font-family:Arial; font-family:Arial">you
 want to unload the driver, just executing the following comma</span><span style="font-family:Arial">nd:</span></span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial; color:#111111"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px"><br style="line-height:1.5">
</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="line-height:18.233333587646484px"><span style="font-family:Courier New; color:#4644FF">[root@localhost anywhere]# rmmod asix</span></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="color:#4644FF">&nbsp; &nbsp; &nbsp;</span><span style="font-family:Arial">----------------</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;Test if the driver is loaded:</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;But How?</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial">==================================</span></div>
<div style="line-height:1.5; margin:0px; padding:0px">
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif">&nbsp;</span><span style="font-family:Arial"> &nbsp; &nbsp; &nbsp; So, I began to configure the Host Manually.</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial"><br style="line-height:1.5">
</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; First, check the USB devices connected on Ubuntu Linux:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&gt; lsusb</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; we get:</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">-----------</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; $ lsusb</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; Bus 001 Device 002: ID 80ee:0021 VirtualBox USB Tablet</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; Bus 001 Device 006: ID 10c4:ea60 Cygnal Integrated Products, Inc. CP210x Composite Device</span></div>
<div style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; Bus 001 Device 007: ID 0b95:7720 ASIX Electronics Corp. AX88772</span></div>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px">
<h2 style="margin:0.8em 0px 0.25em; padding:0px; font-size:18px; line-height:1.2em; color:rgb(17,17,17); font-family:Tahoma,Verdana,Arial,sans-serif">
Manually Setting IP Address</h2>
<p style="margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; line-height:18.233333587646484px; text-align:justify; color:rgb(17,17,17)">
</p>
<div style="text-align:left"><span style="font-family:Arial; line-height:1.5">This section shows what happens inside the script described in section 2 and allow you to follow the</span></div>
<span style="line-height:1.5; margin:0px; padding:0px"></span>
<div style="text-align:left"><span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">setup&nbsp;</span></span><span style="line-height:18.233333587646484px; font-family:Arial">manually.</span></div>
<p></p>
<p style="text-align:left; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; line-height:18.233333587646484px; color:rgb(17,17,17)">
<span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">1. Identify the Ethernet Interface Name inside the Virtual Machine</span></span></p>
<p style="margin-top:0.5em; margin-bottom:0px; margin-left:40px; padding-top:0px; padding-bottom:10px; line-height:18.233333587646484px; text-align:justify; color:rgb(17,17,17)">
</p>
<div style="text-align:left"><span style="font-family:Arial; line-height:1.5">Each time the Ethernet adapter is connected a different name may be associated with it. To list</span></div>
<span style="line-height:1.5; margin:0px; padding:0px"></span>
<div style="text-align:left"><span style="font-family:Arial; line-height:1.5">the currently known Ethernet interfaces, open a new terminal, and execute</span></div>
<p></p>
<pre style="white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); line-height:19px; background-color:rgb(247,249,250); color:rgb(17,17,17)"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px"><span style="font-family:Courier New">&gt; ifconfig</span></span></pre>
<ul style="margin:0px; padding:0px; color:rgb(17,17,17); line-height:18.233333587646484px">
<li style="margin:0px 0px 0px 20px; padding:0px"><span style="font-family:Arial"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">Ifconfig will list two connections.</span>&nbsp;<br style="line-height:1.5; margin:0px; padding:0px">
</span><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp;A) Interface with IP address (this is the connection to the internet), Example output:</span></span></span></li></ul>
<pre style="white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); line-height:19px; background-color:rgb(247,249,250)"><span style="font-family:Courier New"><span style="color:#111111">eth0 Link encap:Ethernet HWaddr 00:0C:29:51:9B:E6&nbsp;</span><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5"><span style="color:#FF0000"><strong><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px"><span style="font-family:Courier New">inet addr</span></span>:</strong></span><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(17,17,17)"><span style="font-family:Courier New">192.168.64.138<span style="margin:0px; padding:0px">&nbsp;</span>Bcast:192.168.64.255 Mask:255.255.255.0</span>&nbsp;<br style="line-height:1.5"><span style="font-family:Courier New"><u>inet6 addr: fe80::20c:29ff:fe51:9be6/64 Scope:Link</u></span>&nbsp;<br style="line-height:1.5"><span style="font-family:Courier New">UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1</span></span></span></pre>
<p style="text-align:left; margin-top:0.5em; margin-bottom:0px; margin-left:40px; padding-top:0px; padding-bottom:10px; line-height:18.233333587646484px; color:rgb(17,17,17)">
<span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">B) Interface without IP address (connection to PAL)</span></span></p>
<p style="text-align:left; margin-top:0.5em; margin-bottom:0px; margin-left:40px; padding-top:0px; padding-bottom:10px; line-height:18.233333587646484px; color:rgb(17,17,17)">
<span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">The device #: 00:50:B6:xx:xx:xx is your Ethernet Adapter #</span></span></p>
<pre style="white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); line-height:19px; background-color:rgb(247,249,250)"><span style="font-family:Courier New"><span style="color:#111111">eth1 Link encap:Ethernet&nbsp;</span><span style="margin:0px; padding:0px"><span style="color:#FF0000">HWaddr 00:50:B6</span></span><span style="color:#111111">:0A:2A:1A&nbsp;</span><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5"><span style="color:#111111"><u>inet6 addr: fe80::20c:29ff:fe51:9be7/64 Scope:Link&nbsp;</u></span><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5"><span style="color:#111111">UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1</span></span></pre>
<p style="text-align:left; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; line-height:18.233333587646484px; color:rgb(17,17,17)">
<span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</span></span><span style="font-family:Arial">The PAL interface can be recognized by&nbsp;</span></p>
<p style="text-align:left; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
&nbsp;</p>
<p style="margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
<span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;(1) a different second line: the line starting with &quot;inet addr&quot; is missing</span><span style="font-family:Arial; line-height:1.5; color:rgb(17,17,17)">&nbsp;(even though the &quot;inet6 addr&quot;<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; may be listed)</span></p>
<p style="margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
<span style="font-family:Arial; color:#111111">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;(2) the HWaddr starts with 00:50:B6.</span></p>
<p style="margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
<span style="font-family:Arial; line-height:1.5; color:rgb(17,17,17)">2. Set IP address for interface connected to PAL</span></p>
<ul style="margin:0px; padding:0px; color:rgb(17,17,17); line-height:18.233333587646484px">
<li style="margin:0px 0px 0px 20px; padding:0px"><span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">Assign IP address by using the following command&nbsp;</span><br style="line-height:1.5; margin:0px; padding:0px">
<span style="font-family:Arial">(we assume&nbsp;<strong>eth1</strong>&nbsp;is the Ethernet connection without specified IP address</span></span><span style="font-family:Arial">)</span></li></ul>
<pre style="white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); line-height:19px; background-color:rgb(247,249,250); color:rgb(17,17,17)"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px"><span style="font-family:Courier New">&gt; sudo ifconfig&nbsp;<strong><u>eth1</u></strong>&nbsp;192.168.40.1</span></span></pre>
<ul style="margin:0px; padding:0px; color:rgb(17,17,17); line-height:18.233333587646484px">
<li style="margin:0px 0px 0px 20px; padding:0px"><span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">&quot;sudo&quot; allows executing a command as root (administrator). &nbsp;For VM pal user, PW:real happy&nbsp;</span></span></li></ul>
<p style="text-align:left; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; line-height:18.233333587646484px; color:rgb(17,17,17)">
<span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">3. Validate that the interface has accepted the IP setting</span></span></p>
<ul style="margin:0px; padding:0px; color:rgb(17,17,17); line-height:18.233333587646484px">
<li style="margin:0px 0px 0px 20px; padding:0px"><span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">Use &quot;ifconfig&quot; again</span></span></li></ul>
<p style="text-align:left; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; line-height:18.233333587646484px; color:rgb(17,17,17)">
<span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">4. Establish a Telnet connection to the board.</span></span></p>
<ul style="margin:0px; padding:0px; color:rgb(17,17,17); line-height:18.233333587646484px">
<li style="margin:0px 0px 0px 20px; padding:0px"><span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">Each PAL is set by default to the IP 192.168.40.211 (you can use &quot;</span><span style="font-family:Courier New">ifconfig</span><span style="font-family:Arial">&quot;
 through minicom to get it)</span></span></li><li style="margin:0px 0px 0px 20px; padding:0px"><span style="line-height:1.5; margin:0px; padding:0px"><span style="font-family:Arial">Open a telnet session by</span></span></li></ul>
<pre style="white-space:normal; margin-top:10px; margin-bottom:10px; margin-left:20px; padding:5px; max-width:80em; overflow:visible; border:1px dashed rgb(187,187,187); line-height:19px; background-color:rgb(247,249,250); color:rgb(17,17,17)"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px"><span style="font-family:Courier New">&gt; telnet 192.168.40.211</span></span></pre>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">For my target PAL board, Username: root, PW:uClinux</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">I Set it succesfully, So the Ethernet connection is configured correctly. That means our driver installation is<br>
Done!</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial">===================== &nbsp;Configure your telnet connection with the target board &nbsp; ====================</span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; color:rgb(17,17,17); margin:0px; padding:0px">
<p style="margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px">
<span style="line-height:1.5; margin:0px; padding:0px"><span style="line-height:18.233333587646484px"><span style="font-family:Arial">Sometimes, when our automatically network connection is not stable, i.e. the DHCP can not work proplerly,</span></span><br style="line-height:1.5">
</span><span style="font-family:Tahoma,Verdana,Arial,sans-serif; line-height:18.233333587646484px">&nbsp;we need the mannually conifigure the ip for our telnet.</span></p>
<p style="line-height:18.233333587646484px; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
<span style="margin:0px; padding:0px"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5; margin:0px; padding:0px; font-size:32px">Manually Configuring Ethernet</span></span></p>
<p style="line-height:18.233333587646484px; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
This section describes how to manually configure the Ethernet connection on SDE 5.3.x. This is an<br>
alternative to&nbsp;running the automated setup script add-trendnet-adapter.py.</p>
<h3 style="line-height:1.2em; margin:0.6em 0px 0.2em; padding:0px; font-size:14px; font-family:Tahoma,Verdana,Arial,sans-serif">
1. Connect TU2-ET100 and make sure it is connected to the Virtual Machine (see instructions&nbsp;<a href="https://nuforge.coe.neu.edu/gf/project/pal/wiki/?pagename=UcLinux&#43;Connection">here</a>)</h3>
<h3 style="line-height:1.2em; margin:0.6em 0px 0.2em; padding:0px; font-size:14px; font-family:Tahoma,Verdana,Arial,sans-serif">
2. Open context menu to to configure connections through NetworkManager.</h3>
<ul style="line-height:18.233333587646484px; margin:0px; padding:0px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
<li style="margin:0px 0px 0px 20px; padding:0px">Click right mouse button on NetworkManager icon</li><li style="margin:0px 0px 0px 20px; padding:0px">Select &quot;Edit Connections ...&quot;</li></ul>
<p style="text-align:left; line-height:18.233333587646484px; margin-top:0.5em; margin-bottom:0px; margin-left:40px; padding-top:0px; padding-bottom:10px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
<img width="211" height="187" alt="Open Context Menu on NetworkManager and select &quot;Edit Connections&quot;" src="https://nuforge.coe.neu.edu/fckeditor/editor/filemanager/img.php?file=2623" style="border:0px; margin:15px; padding:0px; outline:none"></p>
<h3 style="line-height:1.2em; margin:0.6em 0px 0.2em; padding:0px; font-size:14px; font-family:Tahoma,Verdana,Arial,sans-serif">
3. In the dialog box &quot;Network Connections&quot;, select the Wired Connection that corresponds to your USB<br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
Ethernet adapter.</h3>
<ul style="line-height:18.233333587646484px; margin:0px; padding:0px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
<li style="margin:0px 0px 0px 20px; padding:0px">Note which connection has appeared after plugging in the USB Ethernet adapter. The image below,<br>
assumes&nbsp;&nbsp;&quot;eth0&quot; is the connection for the TU2-ET100.</li><li style="margin:0px 0px 0px 20px; padding:0px">Then, select &quot;Edit ...&quot;</li></ul>
<p style="text-align:left; line-height:18.233333587646484px; margin-top:0.5em; margin-bottom:0px; margin-left:40px; padding-top:0px; padding-bottom:10px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
<img width="485" height="332" alt="Select connection for your Ethernet adapter (assumed here eth0)" src="https://nuforge.coe.neu.edu/fckeditor/editor/filemanager/img.php?file=2624" name="image_operate_67751357066918413" style="border:0px; margin:15px; padding:0px; outline:none"></p>
<h3 style="line-height:1.2em; margin:0.6em 0px 0.2em; padding:0px; font-size:14px; font-family:Tahoma,Verdana,Arial,sans-serif">
4. Validate that the TU2-ET100 was correctly selected</h3>
<ul style="line-height:18.233333587646484px; margin:0px; padding:0px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
<li style="margin:0px 0px 0px 20px; padding:0px">The Device MAC address should start with 00:50:b6.</li></ul>
<p style="text-align:left; line-height:18.233333587646484px; margin-top:0.5em; margin-bottom:0px; margin-left:40px; padding-top:0px; padding-bottom:10px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
<img width="449" height="509" alt="Validate MAC address of Connection" src="https://nuforge.coe.neu.edu/fckeditor/editor/filemanager/img.php?file=2625" name="image_operate_33421357066730213" style="border:0px; margin:15px; padding:0px; outline:none"></p>
<h3 style="line-height:1.2em; margin:0.6em 0px 0.2em; padding:0px; font-size:14px; font-family:Tahoma,Verdana,Arial,sans-serif">
5. Select Tab &quot;IPV4 Settings&quot;, and configure the interface.</h3>
<ul style="line-height:18.233333587646484px; margin:0px; padding:0px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
<li style="margin:0px 0px 0px 20px; padding:0px">Under&nbsp;<span style="margin:0px; padding:0px">Method,&nbsp;</span>select &quot;<span style="margin:0px; padding:0px">Manual</span>&quot;</li><li style="margin:0px 0px 0px 20px; padding:0px">Select:&nbsp;<span style="margin:0px; padding:0px">Add</span></li><li style="margin:0px 0px 0px 20px; padding:0px">Enter the following values into line under Addresses
<ul style="margin:0px; padding:0px">
<li style="margin:0px 0px 0px 20px; padding:0px">Address: 192.168.40.1</li><li style="margin:0px 0px 0px 20px; padding:0px">Netmask: 255.255.255.0</li><li style="margin:0px 0px 0px 20px; padding:0px">Gateway: 192.168.40.1</li></ul>
</li><li style="margin:0px 0px 0px 20px; padding:0px">Close &quot;Editing eth0&quot; dialog box by selecting &quot;<span style="margin:0px; padding:0px">Save</span>&quot;
<ul style="margin:0px; padding:0px">
<li style="margin:0px 0px 0px 20px; padding:0px">Save button is grayed out unless valid values are entered for Address.</li></ul>
</li></ul>
<p style="text-align:left; line-height:18.233333587646484px; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
<img width="457" height="508" alt="" src="https://nuforge.coe.neu.edu/fckeditor/editor/filemanager/img.php?file=2628" name="image_operate_72331357066729957" style="border:0px; margin:15px; padding:0px; outline:none"></p>
<h3 style="line-height:1.2em; margin:0.6em 0px 0.2em; padding:0px; font-size:14px; font-family:Tahoma,Verdana,Arial,sans-serif">
6. Close dialog box Network Connections.</h3>
<p style="line-height:18.233333587646484px; margin-top:0.5em; margin-bottom:0px; padding-top:0px; padding-bottom:10px; text-align:justify; font-family:Tahoma,Verdana,Arial,sans-serif; font-size:15px">
</p>
<div style="text-align:left">The Ethernet connection is now configured. The same configuration will be recovered next time</div>
<div style="text-align:left">the same USB Ethernet adapter is connected.</div>
<p></p>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><em><strong><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</strong></em></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><em><strong>================================================</strong></em></span></div>
<div style="line-height:18.233333587646484px; margin:0px; padding:0px"><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; color:rgb(17,17,17)"><span style="font-family:Arial"></span></span>
<p style="font-family:Arial; color:rgb(51,51,51); font-weight:bold; font-style:italic; font-size:14px; line-height:26px">
<span style="color:rgb(17,17,17)">&nbsp;===============================================</span></p>
<p style="font-family:Arial; font-size:14px; line-height:26px"><span style="color:rgb(17,17,17)"><strong>If you want to use the library &quot;libasound.a/out&quot;,&nbsp;<br>
you may need the &quot;libasound2&quot; package.</strong></span><br>
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
the brown text below is my failed configuration note, just skip it ;)<br>
<span style="color:#ff9900">----so, go to Ubuntu website and download the alsa package:<br>
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
<br>
<br>
the &quot;asound&quot; library directly as -lasound, but no assigning the path of it because I have installed the&nbsp;<br>
<br>
<br>
libasound2 package above.<br>
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
&nbsp; &nbsp; &nbsp; &nbsp; Description<span style="white-space:pre"></span>Resource<span style="white-space:pre"></span>Path<span style="white-space:pre"></span>Location<span style="white-space:pre"></span>Type<br>
&nbsp; &nbsp; &nbsp; &nbsp; skipping incompatible /usr/lib/libasound.so when searching for -lasound<span style="white-space:pre"></span>audio_example<span style="white-space:pre"></span><span style="white-space:pre"></span><br>
<br>
<br>
C/C&#43;&#43; Problem<br>
&nbsp; &nbsp; &nbsp; &nbsp; skipping incompatible /usr/lib/libasound.a when searching for -lasound<span style="white-space:pre"></span>audio_example<span style="white-space:pre"></span><span style="white-space:pre"></span><br>
<br>
<br>
C/C&#43;&#43; Problem<br>
&nbsp; &nbsp; &nbsp; &nbsp; cannot find -lasound<span style="white-space:pre"></span>audio_example<span style="white-space:pre"></span><span style="white-space:pre"></span>C/C&#43;&#43; Problem<br>
&nbsp; &nbsp; &nbsp; &nbsp; make: *** [audio_example] Error 1<span style="white-space:pre"></span>audio_example<span style="white-space:pre"></span><span style="white-space:pre"></span>C/C&#43;&#43; Problem<br>
</span><br>
<br>
<span style="color:#111111">----Copy the /opt/ALSA_INCLUDE/alsa/sound &amp; /opt/ALSA_LIB from the Fedora Host System that released by The&nbsp;</span><br>
<br>
<br>
<span style="color:#111111">Learning Labs company. Put them into my U-disk.</span><br>
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
<span style="color:#111111">&nbsp; &nbsp; Ubuntu 12.04下允许Root用户直接登录图形界面:</span><br>
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
<span style="color:#111111">&nbsp; &nbsp; Description</span><span style="color:rgb(17,17,17); white-space:pre"></span><span style="color:#111111">Resource<span style="white-space:pre"></span>Path<span style="white-space:pre"></span>Location<span style="white-space:pre"></span>Type</span><br>
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
<span style="color:#cccccc">At this moment we're only supporting our Fedora based TLL SDE as Design / Development host OS for TLL platforms, this enables us to ensure all the tools/settings/dependencies work seamlessly with each other and thus requires us to
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
<p style="font-family:Arial; color:rgb(51,51,51); font-size:14px; line-height:26px">
<span style="color:rgb(17,17,17)"><br>
</span></p>
<p style="font-family:Arial; color:rgb(51,51,51); font-size:14px; line-height:26px">
<span style="color:rgb(17,17,17)">至此，我配置好了blackfin的开发环境，但是没法配置适用于TLL所有板载功能的开发环境。因为TLL板自己的开发环境为了使用TLL板，有太多的东西需要配置。我并没有完整的配置文档。或许他们公司经过一次次的修改，才发布的SDE本身已经不知道要配置哪些东西了。</span></p>
<p style="font-family:Arial; color:rgb(51,51,51); font-size:14px; line-height:26px">
<span style="color:rgb(17,17,17)">到此为止吧。。。。。</span><span style="color:rgb(204,0,0)"><strong>但是</strong></span><span style="color:rgb(204,0,0)">：</span></p>
<p style="font-family:Arial; color:rgb(51,51,51); font-size:14px; line-height:26px">
<span style="color:rgb(17,17,17)"></span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif"><span style="color:rgb(17,17,17)">=============================================================================<br>
&nbsp; &nbsp;&nbsp;</span><br>
<strong><span style="color:rgb(255,0,0)">&nbsp; &nbsp; But when I compile the accelerometer, there is no errors happened. This<br>
makes me burn a new fire in my mind.</span></strong></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif"><span style="color:rgb(17,17,17)"><strong>&nbsp; &nbsp; I began to test the other nuggets except audio.</strong></span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif"><span style="color:rgb(17,17,17)">=============================================================================</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif"><span style="color:rgb(17,17,17)">&nbsp; &nbsp;&nbsp;&nbsp; &nbsp; OMG!!!<br>
&nbsp; &nbsp; I fixed this problem by: add the libarary -ldl into my objects.mk file which is created<br>
by Eclipse.</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif"><span style="color:rgb(17,17,17)">&nbsp; &nbsp; so:</span></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif"><span style="color:rgb(17,17,17)"></span></p>
<pre name="code" class="plain" style="font-family: 宋体, Verdana, Arial, Helvetica, sans-serif;">    * For My Nuggets On Ubuntu12.04:
    * You need library  &quot;TLL6527M_C_API_uClinux_TLL2012R1DevTEST&quot; at /prerelease20120823/libary/elfStatic/
    * You need include  /prerelease2012xxxx/inclue/common/
    *
    * For Audio:
    * You need library  &quot;dl&quot; (No need to assign its path, system default.)
    * You need library  &quot;asound&quot; from /opt/ALSA_LIB
    * You need include  .../prerelease2012xxxx/inclue/audio/
    * You need include  /opt/ALSA_INCLUDE/alsa/sound</pre><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif">=====================================================</span>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif"></p>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif">遇到 Eclipse 编译时出现：<br>
undefined reference to '_dlclose'<br>
undefined reference to '_dlopen'<br>
undefined reference to '_dlsym'<br>
undefined reference to '_dlsym'<br>
我的解决办法：<br>
&nbsp; &nbsp; 我是修改了.mk 文件，实际上是增加了一个库， dl (dynamic link)&nbsp;<br>
&nbsp; &nbsp; 在原本描述链接库的地方:</p>
<pre name="code" class="plain" style="font-family: 宋体, Verdana, Arial, Helvetica, sans-serif;">LIBS := -lTLL6527M_C_API_uClinux_TLL2012R1DevTEST -lasound</pre><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif">&nbsp; &nbsp; 修改为:&quot;</span><pre name="code" class="plain" style="font-family: 宋体, Verdana, Arial, Helvetica, sans-serif;">LIBS := -lTLL6527M_C_API_uClinux_TLL2012R1DevTEST -lasound -ldl</pre><span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif">再重新编译，就没有错误产生了。</span><br>
<span style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif">===================================================== &nbsp; &nbsp;</span>
<p style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif"></p>
<p><span style="font-family:Arial">&nbsp; &nbsp; But I met the errors when I launch the app bin file on my target board! :(</span></p>
<p><span style="font-family:Arial">&nbsp; &nbsp; See the log below:</span></p>
<p></p>
<pre name="code" class="plain"><span style="font-family:Courier New;">root:/home&gt; ls
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
root:/home&gt;</span></pre><span style="font-family:Arial"><br>
&nbsp; &nbsp; I really do not know how to fix the alsa-lib problem....</span>
<p></p>
<span style="font-family:Arial"><br>
<br>
</span></div>
<div style="line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<em><strong><span style="font-family:Arial"><br style="line-height:1.5">
</span></strong></em></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><em><strong><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</strong></em></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><em><strong><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</strong></em></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><em><strong><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</strong></em></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<div style="margin:0px; padding:0px"><span style="font-family:Arial">================================================</span></div>
<div style="margin:0px; padding:0px"><span style="font-family:Arial">&nbsp; &nbsp; &nbsp; &nbsp; 参考：</span><a href="http://blog.csdn.net/cp1300/article/category/1209321" target="_blank">【嵌入式Linux开发环境配置】</a></div>
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><em><strong><br style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:1.5">
</strong></em></span></div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
</div>
<div style="font-family:宋体,Verdana,Arial,Helvetica,sans-serif; line-height:18.233333587646484px; color:rgb(17,17,17); margin:0px; padding:0px">
<span style="font-family:Arial"><em><strong>&nbsp;</strong></em></span></div>
</div>
</div>
</div>
   

---
layout: post
title: '有关Firefox/Chrome的问题汇总'
date: 2013-11-27 11:27:00 +0800
category: from_cnblogs
slug: p20131127112700
---


<p><span style="line-height:27px"><br>
</span><span style="line-height:27px"></span></p>
<div class="for">
<h1 id="w_ihgoceoeoeoakekiuauekaeugaauce-firefox-daeekunagcasinoz">安装的附加组件因未经验证而被 Firefox 禁用，我该怎么办</h1>
<p>如果您已安装的附加组件因未经验证而被禁用了，建议您联系附加组件开发者或提供给您附加组件的人，看看他们能不能提供新版经过验证的附加组件。您也可以要求他们<a target="_blank" href="https://developer.mozilla.org/en-US/Add-ons/Distribution">将附加组件提交验证</a>。</p>
</div>
<div class="for">
<div class="note"><strong>强制禁用这个首选项（高级用户）：</strong> 你可以在 <a target="_blank" href="https://support.mozilla.org/zh-CN/kb/about-config-editor-firefox">
Firefox 配置编辑页面</a> （<em>about:config</em> 页面）把 <span class="pref">xpinstall.signatures.required</span> 首选项设为
<strong>false</strong> 来强制禁用附加组件验证的功能。我们不会为手动在配置编辑页面做出的任何修改提供援助，因此请自担风险。</div>
</div>
<br>
<span style="line-height:27px"><br>
</span>
<p></p>
<h2><span style="line-height:27px">更新 Chrome 后，新标签下方的最近访问菜单消失-【解决】</span></h2>
<span style="line-height:27px"><br>
How to find back the disappeared &quot;recently closed&quot; bar under new tab in Chrome which had been upgrated.<br>
<br>
Reopen recently closed tabs in Chrome<br>
If you have just closed a tab by mistake or need to re-visit a recently closed website do not panic - all is not lost.<br>
<br>
</span><span>Have you ever wished you hadn't just closed that Google Chrome browser tab. Get it back by right-clicking in the tab bar and selecting&nbsp;</span><span span=""><strong>Ctrl&#43;Shift&#43;T</strong></span><span>.</span>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; color:rgb(51,51,51)">
</p>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; color:rgb(51,51,51)">
You can also find a list of recently closed tabs on the &quot;new tab&quot; screen in Google Chrome. Click on&nbsp;<strong>Recently closed</strong>&nbsp;to the right of the bottom bar to open a list of your most recent tabs.</p>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; color:rgb(51,51,51)">
Finally, if you always want to start where you left off last time when opening Chrome, go to Settings by clicking on the far right icon (3 horizontal lines) and choose Settings from the menu. In the Start Up section choose&nbsp;<strong>Continue where I left off&nbsp;</strong>then
 close the tab. Next time you open Chrome you will get the open tabs from your last browsing session.</p>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; color:rgb(51,51,51)">
Do you like these tips, do you have any more to share? Let us know in the comments.</p>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; color:rgb(51,51,51)">
<em>TIP UPDATE:</em>&nbsp;The recent Chrome update has moved recently closed tabs to the Settings menu. Click on the 3 horizontal line icon top right and choose&nbsp;<strong>Recent tabs</strong>.</p>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; color:rgb(51,51,51)">
<br>
</p>
<h4>------ &nbsp;这里要说的是一个更专业的方法 ------</h4>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; color:rgb(51,51,51)">
<span style="color:rgb(34,34,34)"><strong>1. Go to&nbsp;</strong>chrome://flags</span></p>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; color:rgb(51,51,51)">
<span style="color:rgb(34,34,34)"><strong>2. Find:</strong></span><br style="color:rgb(34,34,34)">
<span style="color:rgb(34,34,34)">Enable Instant Extended API Mac, Windows, Chrome OS&nbsp;<br>
</span><span style="color:rgb(34,34,34); font-family:Arial,Helvetica,sans-serif">Enables the Instant Extended API which provides a deeper integration with your default search provider, including a renovated New Tab Page, extracting search query terms in the
 omnibox, a spruced-up omnibox dropdown and Instant previews of search results as you type in the omnibox.&nbsp;</span></p>
<p><span style="color:rgb(34,34,34); font-size:13px"><strong>3. Set it to '</strong>Disabled<strong>'.</strong></span></p>
<p><span style="color:rgb(34,34,34); font-size:13px">You'll sacrifice the other features along with the updated New Tab Page, but I think it is worth it to not be punished because some users can't figure out how to use the omnibar. &nbsp;Lowest common denominator
 BS.</span></p>
<p>Refer to:<a target="_blank" target="_blank" href="http://productforums.google.com/forum/#!topic/chrome/KSa1CJ9aoEc%5B1-25-true%5D">http://productforums.google.com/forum/#!topic/chrome/KSa1CJ9aoEc%5B1-25-true%5D</a></p>
<p><br>
</p>
<p><br>
</p>
<h2>2. 如何禁止Google Chrome Browser Update:</h2>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; line-height:18px; color:rgb(51,51,51)">
2015年7月28日23:45:43<br>
最近发现Chrome V43 不支持NPAPI，导致很多淘宝和视频网站必须的应用程序插件无法运行，找到v41的 offline installer, 决定安装完了disable chrome update<br>
参考：http://stackoverflow.com/questions/18483087/how-can-i-disable-google-chrome-auto-update</p>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; line-height:18px; color:rgb(51,51,51)">
<span style="font-weight:bold">For Windows only</span></p>
<p style="margin-top:0px; margin-bottom:9px; font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px; line-height:18px; color:rgb(51,51,51)">
</p>
<div class="post-text">
<p>To completely disable automatic Chrome updates create/edit the following registry keys/dwords under<code>HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Google\Update</code>:</p>
<ul>
<li>Dword: <code>AutoUpdateCheckPeriodMinutes</code>Value: <code>0</code></li><li>Dword: <code>DisableAutoUpdateChecksCheckboxValue</code>Value: <code>1</code></li><li>Dword: <code>UpdateDefault</code>Value: <code>0</code></li><li>Dword: <strong><code>Update{8A69D345-D564-463C-AFF1-A69D9E530F96}</code></strong>Value:<code>0</code>
</li></ul>
<p><strong>This last one was the money for me.</strong> Until I added it updates were enabled in<code>chrome://chrome</code>.</p>
<p><strong>Note:</strong> On 64-bit machines running 32-bit Chrome you may need to put the dwords under the following key instead:<code>HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Policies\Google\Update</code>.</p>
<p><br>
</p>
</div>
<div style="top:0px">
<div><span style="font-size:14px"><br>
</span></div>
</div>
   

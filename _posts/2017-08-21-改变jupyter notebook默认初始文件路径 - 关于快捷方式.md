---
layout: post
title: '改变jupyter notebook默认初始文件路径 - 关于快捷方式'
date: 2017-08-21 03:08:00 +0800
category: from_cnblogs
slug: p20170821030800
---
<h2>jupyter notebook home path changing - %USERFROFILE% and Configure file</h2>
<p>如何改变jupyter notebook默认初始文件路径，网上都提供了很多方法。<a href="http://blog.csdn.net/lixintong1992/article/details/53012921" target="_blank">Link1</a> , <a href="http://blog.csdn.net/qq_33039859/article/details/54604533" target="_blank">Link2</a></p>
<p>但他们都没让你这样测试：</p>
<blockquote>
<p style="margin-left: 30px;">[Win] + R , "cmd", 在windows命令行运行：</p>
<p style="margin-left: 30px;">C:/ProgramData/Anaconda3/python.exe C:/ProgramData/Anaconda3/Scripts/jupyter-notebook-script.py</p>
<p style="margin-left: 30px;"><span style="color: #800000;">注：如果这样测试可以修改，而通过开始菜单双击&ldquo;Jupyter Notebook&rdquo;快捷方式还是没改过来，那就往下看。</span></p>
</blockquote>
<p>他们都忽略了一个问题：在 Jupyter Notebook 快捷方式 - 属性 - Shortcut Tag- Target 栏内的内容，会影响你修改的效果。</p>
<p>即：可能你已经改了以下文件：</p>
<ul>
<li>C:\Users\&lt;username&gt;\.jupyter\jupyter_notebook_config.py</li>
<li>并把快捷方式里Start In内改成了你要的path: C:\Users\&lt;username&gt;\&lt;your_preferred_path&gt;</li>
</ul>
<p>还是可能不生效，原因就是 Target 里的%USERFROFILE%或别的内容在使改动不生效。</p>
<p>我的Target内内容：</p>
<blockquote>
<p>"C:/ProgramData/Anaconda3/python.exe" "C:/ProgramData/Anaconda3/Scripts/jupyter-notebook-script.py"</p>
</blockquote>
<p>-----</p>
<p>Nov 2, 2017 Updated:</p>
<p>1. Install Anaconda 5<br />2. add the path of "jupyter notebook.exe" into Environment Varialble: "path"<br />   make sure you can launch jupyter notebook by: launching cmd command: "jupyter notebook"<br />3. in cmd window, cd to the path that you want to create the configure file "jupyter_notebook_config.py"<br />   you will get: Writing default config to: C:\Users\Administrator\.jupyter\jupyter_notebook_config.py"<br />4. modify the config file "jupyter_notebook_config.py" at line of "#c.NotebookApp.notebook_dir ='' "<br />   as:<br />   c.NotebookApp.notebook_dir = '&lt;path you prefer&gt;'<br />5. now you can launch jupyter notebook via cmd window. But, the shortcut is still not like you want.</p>
<p>---<br />6. modify the content in "target" in your properties of your jupyter_notebook's shortcut:<br />   "C:/ProgramData/Anaconda3/python.exe" "C:/ProgramData/Anaconda3/Scripts/jupyter-notebook-script.py"</p>
<p>&nbsp;OK.</p>
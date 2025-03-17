---
layout: post
title: 'Ruby学习笔记3：Rendering（渲染）和 Redirect（重定向）'
date: 2015-05-06 14:05:00 +0800
category: from_cnblogs
slug: p20150506140500
---


<p><br>
</p>
<h3>1. Rendering<br>
</h3>
<p>Rendering 是特别要告诉Controller 中的methods，要哪个view file来显示给用户。We can show Views as we wish!</p>
<p></p>
<p>Earlier each one of our Views rendered based on the method specified in the Controller.</p>
<p>If we write the following method:</p>
<pre><code class="lang-rb"><span class="function"><span class="keyword">def</span> <span class="title">render_demo</span></span>
<span class="identifier"><span class="keyword">end</span></span>
</code></pre>
<p>Rails will always look for the render_demo View. But if we write:</p>
<pre><code class="lang-rb"><span class="function"><span class="keyword">def</span> <span class="title">render_demo</span></span>
  <span class="identifier">render</span> <span class="symbol">:<span class="identifier">home</span></span>
<span class="identifier"><span class="keyword">end</span></span>
</code></pre>
<p>We can tell Rails to render the home View, as long as we have one. Rendering just tells Rails to show the View we specify.</p>
这里这个例子是这样的：<br>
1. 在pages_controller.rb文件中，加入一个method: render_demo<pre code_snippet_id="660614" snippet_file_name="blog_20150506_1_4172159"  code_snippet_id="660614" snippet_file_name="blog_20150506_1_4172159" name="code" class="ruby">class PagesController &lt; ApplicationController
   def home
   end
   def erb_demo
   end
   def render_demo
      render :home  #return home page. but return render_demo.html.erb if without this line
   end
&#160;end
</pre>2. 在routes.rb文件中，加入一个route：pages/render_demo<pre code_snippet_id="660614" snippet_file_name="blog_20150506_2_8660352"  code_snippet_id="660614" snippet_file_name="blog_20150506_2_8660352" name="code" class="ruby">Rails.application.routes.draw do
  get &#39;pages/home&#39;
  root &#39;pages#home&#39;
  get &#39;pages/erb_demo&#39;
  get &#39;pages/render_demo&#39;
end</pre>3. 在render_demo.html.erb文件中:<pre code_snippet_id="660614" snippet_file_name="blog_20150506_3_2724218"  code_snippet_id="660614" snippet_file_name="blog_20150506_3_2724218" name="code" class="ruby">  &lt;%= &quot;This is the render demo template.&quot; %&gt;
</pre>4. 最后在浏览器中请求： localhost:8000/pages/render_demo&nbsp;&nbsp;&nbsp; ，可以得到homepage. 或得到&quot;This is the render
 demo template&quot;<br>
<p><br>
</p>
<p>---------------------</p>
<h3>2. redirect<br>
</h3>
<p>另一个控制view内容的方法是redirect, 它跟rendering有点类&#20284;，不过它是重新发送一次request到一个different URL. redirect显示了一个不同的view,Redirecting 产生了一个全新的request.<br>
<br>
下面就是我们怎么在rails中使用redirect：<br>
1. 在pages_controller.rb文件中，加入一个method: <br>
<span style="font-family:Courier New"><span style="background-color:rgb(204,204,204)">def redirect_demo<br>
&nbsp;&nbsp; redirect_to(:action =&gt; 'home')<br>
end</span></span><br>
<br>
2. 在routes.rb 文件中，加入一个route: pages/redirect_demo<br>
<br>
3. 在 redirect_demo.html.erb 文件中，代码如下：<br>
<span style="background-color:rgb(204,204,204)"><span style="font-family:Courier New">&lt;%= &quot;This is your redirect demo template&quot; %&gt;</span></span><br>
</p>
<p>----------------------</p>
<p>到这里我们就掌握了利用Rails建立一个基本的Static Web App --- 静态web应用<br>
</p>
<p>接下来我们要接触动态应用。加油！<br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
   

---
layout: post
title: 'Ruby学习笔记2 ： 一个简单的Ruby网站，搭建ruby环境'
date: 2015-05-05 02:54:00 +0800
category: from_cnblogs
slug: p20150505025400
---


<p>Ruby on Rails website 的基础是 请求-返回 循环。<br>
<br>
<br>
首先是浏览器请求服务器，<br>
<br>
<br>
第二步，Second, in our Rails application, the route takes the request and finds the right Controller and method to handle the request.<br>
<br>
<br>
第三步，控制器拿到相关数据并发给View模块。<br>
<br>
<br>
最后，View模块把数据打包好，并且发回给浏览器显示。</p>
<p>-----</p>
<h3>搭建开发环境：<br>
</h3>
<ul>
<li>Creat rails file package: rails new my-app</li><li>Install Ruby gems</li><li>Making a controller called pages<br>
</li><li>在routes.rb文件中，Create a route</li><li>Create the View<br>
</li></ul>
<h3>1. Creat rails file package<br>
</h3>
<p>操作 Rails 用的commands叫做Generator， 在Terminal里，使用如下命令可以新建一个rails app:</p>
<p></p>
<pre><code>rails <span class="keyword">new</span> my-app</code></pre>
rails就会建立一系列的站点文件。<br>
然后我们在浏览器里输入：localhost:8000就能访问rails建立的第一个web 站点：
<p></p>
<p><img src="http://img.blog.csdn.net/20150505151242538?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<p>&nbsp; Let's learn about the files that you generated.来看看我们建立了哪些文件：</p>
<p>&nbsp; Rails is made up folders that hold our code:</p>
<p><code>&nbsp;&nbsp; <span style="background-color:rgb(204,204,204)">app</span></code> - Where our Models, Views and Controllers are. It is where most of our Rails code will go.</p>
<p><code>&nbsp;&nbsp; <span style="background-color:rgb(204,204,204)">config</span></code> - Where we find our routes. They handle requests from the browser.</p>
<p><code>&nbsp;&nbsp; <span style="background-color:rgb(204,204,204)">db</span></code> - Where we change the state of our database.</p>
<p><code>&nbsp;&nbsp; <span style="background-color:rgb(204,204,204)">Gemfile</span></code> - Where our gems are. Gems add special features to our Rails app.<br>
<br>
可以参考下图：<br>
</p>
<p><img src="http://img.blog.csdn.net/20150505152100645?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br>
</p>
<p><br>
</p>
<p></p>
<h3>2. Install Gem</h3>
<p></p>
<p>To run Rails we also need to install Ruby gems. Gems are prepackaged code that make sure our application runs smoothly. They can also add new features.</p>
<p></p>
<p>We add gems through a simple command called bundler. <strong>Bundler</strong> takes the gems in our Gemfile, an editable file, and stores them in Gemfile.lock, an uneditable file.</p>
<p>To run bundler, we type the following in our terminal:</p>
<p><span style="background-color:rgb(204,204,204)"><span style="font-family:Courier New">bundle install</span></span><br>
</p>
&nbsp; <br>
<p></p>
<h3>3. Making a controller called pages<br>
</h3>
<p></p>
<p></p>
<div class="accordion-section-body">
<div class="accordion-section-body__padded narrative--markdown">
<p>In the Request-Response Cycle, we learned that when a user makes a request in the browser, the route handles that request and passes it to the right Controller.</p>
<p>一个route路由来处理浏览器的request,请求。发给Controller<br>
</p>
<p>We’ll <strong>start building our app by making a Controller called Pages</strong>. Controllers decide what information our application needs through methods. Our Pages Controller will be in charge of decisions for the home page of the Etsy app.</p>
<p>How do we generate a Controller?</p>
<p>If we want to generate a Controller called <code>Pages</code>, we enter the following in the terminal:</p>
<pre><code>rails generate controller Pages
</code></pre>
<p>Notice that Controller names are always plural.</p>
<p>现在我们就有了pages controller(<span>app/controllers/pages_controller.rb</span>)，控制器是通过methods来决策的，这些methods跟前面用的ruby methods是相&#20284;的，同时，它们是写在我们创建的Pages Controller里的。<br>
<br>
Controller methods主要作用于两方面：<br>
a -- 告诉我们的app，应该显示哪个view<br>
b -- 准备信息，让view可以轻易地显示它。<br>
<br>
如果是静态的app，methods就简单了，它只需要告诉Rails哪个view to show.例如：<br>
<span style="background-color:rgb(204,204,204)">&nbsp;&nbsp;&nbsp; def home<br>
&nbsp;&nbsp;&nbsp; end</span><br>
这就是告诉controller来render home view. 如果methods里有内容，就会执行它们。这里我们只需要def home 和 end ，Rails就知道是要call the home view.<br>
在<span>app/controllers/pages_controller.rb</span>文件中：</p>
<pre code_snippet_id="660413" snippet_file_name="blog_20150506_1_4763652"  code_snippet_id="660413" snippet_file_name="blog_20150506_1_4763652" name="code" class="ruby">class PagesController &lt; ApplicationController
   def home
   end
end</pre><br>
操作：
<p></p>
<p></p>
<div class="fcn-checkpoint fcn-checkpoint--satisfied"><strong class="fcn-checkpoint__number"><span>1</span><span>.</span></strong>
<div class="fcn-checkpoint__body"><span></span>
<p>In your Pages Controller, pages_controller.rb, Define a method called <code>def home</code>. You do not need a method body. Hit Run.</p>
</div>
</div>
<strong class="fcn-checkpoint__number"><span>2</span><span>.</span></strong>
<div class="fcn-checkpoint__body"><span></span>
<p>In your browser visit localhost:8000/pages/home to see what was created. You can expect to get a Routing Error.</p>
</div>
<br>
<p></p>
</div>
</div>
=====================
<p></p>
<h3>4. 在routes.rb文件中，Create a route<br>
</h3>
<p>上一步完成后，我们得到了一个Routing Error的错误，我们前面看到了，来自浏览器的request 是需要一个route进行分发的，分发给controller进行处理。所以我们现在要增加一个routes.rb的文件来补充这个route.<br>
<br>
在rails 中，Routes就像app的门一样，来保证正确的分发请求，从而让我们browser 拿到正确的view 返回。<br>
<br>
尽管Routes 是我们的第一站，但是我们还是要先拿到controller里的名字和method名字才能写routes。<br>
<br>
在routes.rb文件中，we create a route like this, under <br>
Rails.application.routes.draw do:<br>
</p>
<pre><code class="lang-rb"><span class="comment">get 'pages/home'</span>
</code></pre>
<br>
这里，pages 指的就是controller pages. &nbsp;&nbsp; home就是指的我们在Controller里定义的那个method的名字home。<br>
<p></p>
<p></p>
<p>在这个事儿里，我们还要创建一个根路径。根路径就是设置一个home page的路径，作为一个app的首页请求。这里， pages/home是我们的首页，所以我们建立的根路径就是：<span style="font-family:Courier New"><span style="background-color:rgb(204,204,204)">root ’pages#home'</span></span> (We will also create a root route in this unique
 case. A root route is a way to set a route as the home page of the application, or the 'root'. Since pages/home will be our home page, we'll create a root route like this:)</p>
<pre><code class="lang-rb">root 'pages<span class="comment">#home'</span>
</code></pre>
<p>然后在浏览器中再输入 localhost:8000&nbsp; 就能预览效果了。<br>
</p>
<p>routes.rb文件源码：</p>
<pre code_snippet_id="660413" snippet_file_name="blog_20150506_2_1760102"  code_snippet_id="660413" snippet_file_name="blog_20150506_2_1760102" name="code" class="ruby">Rails.application.routes.draw do
   get &#39;pages/home&#39;
   root &#39;pages#home&#39;
end</pre><br>
<p></p>
<h3>5. Create the View<br>
</h3>
<p>现在我们已经能够显示一个简单的主页了，不过这个页面只有一句话。我们建立了Controller &amp; Routes, 下一步要Create View了：</p>
<p><img src="http://img.blog.csdn.net/20150506164955438?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<p><strong>Views用来处理用户看到的可见的显示。</strong><br>
Views是怎么来管理presentation的呢？<br>
1.通过html来管理页面。<br>
2.通过ERB - Embedded Ruby ，它是把ruby代码嵌入html页面的一种途径。We can use Ruby objects in our HTML files just as we do in plain Ruby.<br>
<br>
由于Views使用了html and ERB，所以View的files总是后缀为.html.erb<br>
<br>
在.html.erb文件中，主要有2个categories of ERB: 一个是只在我们代码里显示的code,一种是显示在app里的live code.<br>
<br>
如果我们写成：<br>
&nbsp; &lt;% &quot;goodbye world&quot; %&gt;<br>
&nbsp;&nbsp; &nbsp;就只有后台能看到。<br>
<br>
如果我们写成：<br>
&nbsp; &lt;%= &quot;hello world&quot; %&gt;<br>
&nbsp;&nbsp; &nbsp;it will appear live in on our app.就一个=号的区别。<br>
<br>
这里这个例子是这样的：<br>
1. 在pages_controller.rb文件中，加入一个method: erb_demo</p>
<pre code_snippet_id="660413" snippet_file_name="blog_20150506_3_4395508"  code_snippet_id="660413" snippet_file_name="blog_20150506_3_4395508" name="code" class="ruby">class PagesController &lt; ApplicationController
   def home
   end
   def erb_demo
   end
end
</pre>2. 在routes.rb文件中，加入一个route：pages/erb_demo<pre code_snippet_id="660413" snippet_file_name="blog_20150506_4_5918098"  code_snippet_id="660413" snippet_file_name="blog_20150506_4_5918098" name="code" class="ruby">Rails.application.routes.draw do
get &#39;pages/home&#39;
root &#39;pages#home&#39;
get &#39;pages/erb_demo&#39; #浏览器就可请求localhost:8000/pages/erb_demo,不过，代码里pages/erb_demo是：ControllerName/MethodsName
end</pre>3. 在erb_demo.html.erb文件中，加入两行categories of ERB, 一个不可见，一个可见。<pre code_snippet_id="660413" snippet_file_name="blog_20150506_5_9359517"  code_snippet_id="660413" snippet_file_name="blog_20150506_5_9359517" name="code" class="ruby">  &lt;%= &quot;This is the erb demo template.&quot; %&gt;
  &lt;% &quot;goodbye world&quot; %&gt;
  &lt;%= &quot;hello world&quot; %&gt;</pre>4. 最后在浏览器中请求： localhost:8000/pages/erb_demo&nbsp;&nbsp;&nbsp; ，可以得到：
<p></p>
<p><img src="http://img.blog.csdn.net/20150506172725258?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<p>至此，一个ruby环境的搭建，建立一个简单的ruby app来显示一个网页，就搞定了。<br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
<p><br>
</p>
   

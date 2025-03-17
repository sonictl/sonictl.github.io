---
layout: post
title: 'Ruby学习笔记7: 添加身份验证（adding Authentication）'
date: 2015-05-21 05:47:00 +0800
category: from_cnblogs
slug: p20150521054700
---


<p><br>
</p>
<p>我们已经完成了Category &amp; Product页面内容的增删改查，再加入一个身份验证即可成为一个较完整的Rails App了。本文就来完成这个任务。<br>
We now need to give users the ability to <strong>sign up</strong> for the app so that they can do things like purchase products or leave reviews.<br>
To do this, we'll <strong>add a user authentication system</strong> to the app.看下图：<br>
</p>
<p><img src="http://img.blog.csdn.net/20150521134933961?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""></p>
<h3>1. install a gem called Devise.</h3>
<p></p>
<div class="accordion-section-body">
<div class="accordion-section-body__padded narrative--markdown">
<p>Earlier, we learned about bundler and how it sets up our gems. To add authentication, we'll install a gem called Devise.</p>
<p>We've gone ahead and added Devise for you in your Gemfile. Run bundle to install it in your app.</p>
</div>
</div>
<div class="accordion-section-heading"><span><span class="fcn-icon fcn-icon-instructions"></span><span>【Instructions】</span></span></div>
<strong class="fcn-checkpoint__number"><span>1</span><span>.</span> </strong>In your terminal, run
<span style="color:#339999"><code>bundle install</code></span> to update all your gems. Press Enter.
<p></p>
<p>这个命令看起来好像是依托于一个叫做GemFile的文件的：</p>
<pre code_snippet_id="671999" snippet_file_name="blog_20150521_1_8618631"  name="code" class="plain">source &#39;https://rubygems.org&#39;


# Bundle edge Rails instead: gem &#39;rails&#39;, github: &#39;rails/rails&#39;
gem &#39;rails&#39;, &#39;4.1.1&#39;
# Use sqlite3 as the database for Active Record
gem &#39;sqlite3&#39;, &#39;1.3.9&#39;
# Use SCSS for stylesheets
gem &#39;sass-rails&#39;, &#39;4.0.3&#39;
# Use Uglifier as compressor for JavaScript assets
gem &#39;uglifier&#39;, &#39;1.3.0&#39;
# Use CoffeeScript for .js.coffee assets and views
gem &#39;coffee-rails&#39;, &#39;4.0.0&#39;
# See https://github.com/sstephenson/execjs#readme for more supported runtimes
# gem &#39;therubyracer&#39;,  platforms: :ruby

gem &#39;paperclip&#39;, &#39;4.2.0&#39;

# Use jquery as the JavaScript library
gem &#39;jquery-rails&#39;, &#39;3.1.2&#39;
# Turbolinks makes following links in your web application faster. Read more: https://github.com/rails/turbolinks
gem &#39;turbolinks&#39;, &#39;2.4.0&#39;
# Build JSON APIs with ease. Read more: https://github.com/rails/jbuilder
gem &#39;jbuilder&#39;, &#39;2.2.2&#39;
# bundle exec rake doc:rails generates the API under doc/api.
gem &#39;sdoc&#39;, &#39;0.4.0&#39;,          group: :doc

# Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
gem &#39;spring&#39;, &#39;1.1.3&#39;,       group: :development

# Use ActiveModel has_secure_password
# gem &#39;bcrypt&#39;, &#39;~&gt; 3.1.7&#39;

# Use unicorn as the app server
# gem &#39;unicorn&#39;

# Use Capistrano for deployment
# gem &#39;capistrano-rails&#39;, group: :development

# Use debugger
# gem &#39;debugger&#39;, group: [:development, :test]

gem &#39;rspec&#39;, &#39;3.1&#39;
gem &#39;rspec-rails&#39;, &#39;3.1&#39;
gem &#39;rspec-context-private&#39;, &#39;0.0.1&#39;
gem &#39;rspec-html-matchers&#39;, &#39;0.6.1&#39;

gem &#39;devise&#39;, &#39;3.4.0&#39;
</pre>
<p>运行那个命令以后就进入第二步。</p>
<p>以下是运行的部分结果打印：<pre code_snippet_id="671999" snippet_file_name="blog_20150521_2_8034132"  name="code" class="plain">$ bundle install
Fetching gem metadata from https://rubygems.org/..........
Resolving dependencies...
Using rake 10.3.2
Using i18n 0.6.11
....</pre><br>
<br>
</p>
<h3>2. Create configuration files - for setup</h3>
<p></p>
<div class="accordion-section-body">
<div class="accordion-section-body__padded narrative--markdown">
<p>Devise also comes with a generator that allows us to configure the gem. Devise附带了一个generator，我们可以用它来配置the gem.This is similar to how we generated our Model for Categories and Products. This time we'll be using Devise's built in setup feature.<br>
这次我们要用Devise的自带的setup feature.<br>
</p>
</div>
</div>
<div class="accordion-section-heading"><span><span class="fcn-icon fcn-icon-instructions"></span><span>[Instructions]</span></span></div>
<strong class="fcn-checkpoint__number"><span>1</span><span>.</span> </strong>In your terminal,
<code>rails generate devise:install</code> which will create configuration files for us. Press Enter.<br>
<p></p>
<pre code_snippet_id="671999" snippet_file_name="blog_20150521_3_6478521"  name="code" class="plain">$ rails generate devise:install
create  config/initializers/devise.rb
create  config/locales/devise.en.yml
===============================================================================
$ </pre><br>
<h3>3. Generate devise for user model and routes<br>
</h3>
<p></p>
<div class="accordion-section-body">
<div class="accordion-section-body__padded narrative--markdown">
<p>In order to let users sign up for the app, we need a place to safely store their information.<br>
我们在前面也见到这个问题，当我们需要一个地方来store category &amp; product信息时，我们建立了Category &amp; Product Models. 这里是一样的，把信息存在User Model里。devise 跟了一个命令就是拿来建立user model的，下面就要来了：</p>
</div>
</div>
<div class="accordion-section-heading"><span><span class="fcn-icon fcn-icon-instructions"></span><span>【Instructions】</span></span></div>
<strong class="fcn-checkpoint__number"><span>1</span><span>.</span> </strong>In your terminal, type
<span style="background-color:rgb(204,204,204)"><code>rails generate devise user</code></span> which will create our user model. Press Enter.<pre code_snippet_id="671999" snippet_file_name="blog_20150521_4_4399523"  name="code" class="plain">$ rails generate devise user
invoke  active_record
create    db/migrate/20141015022653_devise_create_users.rb
create    app/models/user.rb
invoke    test_unit
create      test/models/user_test.rb
create      test/fixtures/users.yml
insert    app/models/user.rb
 route  devise_for :users
$ </pre>In addition to creating a User model, Devise also created a route to sign up new users. Let's see what
 this route looks like.<br>
***file:**<span>config/routes.rb</span>:<pre code_snippet_id="671999" snippet_file_name="blog_20150521_5_9255862"  name="code" class="ruby">Rails.application.routes.draw do
  get &#39;/&#39; =&gt; &#39;pages#home&#39;

  resources :categories
  get &#39;categories/:id/delete&#39; =&gt; &#39;categories#delete&#39;, :as =&gt; :categories_delete

  resources :products
  get &#39;products/:id/delete&#39; =&gt; &#39;products#delete&#39;, :as =&gt; :products_delete

  devise_for :users  #the routes for users created by advise
end</pre><br>
<br>
<h3>4. Migrate database<br>
</h3>
<div class="accordion-section-body">
<div class="accordion-section-body__padded narrative--markdown">
<p>In our Model, we can see that Devise adds a number of words ending in 'able'. These are different functionalities that we can add to our app, like registering new users and remembering them.</p>
<p>In our Migration file, we can see that Devise has added new columns for each module it created. The Migration table works similar to the ones we created earlier, storing new features.<br>
*** file: <span>app/models/user.rb</span>:<pre code_snippet_id="671999" snippet_file_name="blog_20150521_6_175487"  name="code" class="ruby">class User &lt; ActiveRecord::Base
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
end</pre><br>
</p>
</div>
</div>
<div class="accordion-section-heading"><span><span>[Instructions]</span></span></div>
<strong class="fcn-checkpoint__number"><span>1</span><span>.</span></strong>In your terminal, run
<span style="background-color:rgb(204,204,204)"><code>bundle exec rake db:migrate</code></span> to migrate your database. Press Enter.<br>
<pre code_snippet_id="671999" snippet_file_name="blog_20150521_7_6144641"  name="code" class="plain">$ bundle exec rake db:migrate
== 20140929235235 DeviseCreateUsers: migrating ================================
-- create_table(:users)
   -&gt; 0.0055s
-- add_index(:users, :email, {:unique=&gt;true})
   -&gt; 0.0008s
-- add_index(:users, :reset_password_token, {:unique=&gt;true})
   -&gt; 0.0025s
== 20140929235235 DeviseCreateUsers: migrated (0.0090s) =======================
</pre>
<p></p>
<h3>5. Create our First User</h3>
<div class="accordion-section-body">
<div class="accordion-section-body__padded narrative--markdown">
<p>Now that the User Model is set up, let's create our first user. This is similar to what we did earlier for Products and Categories.<br>
We can do this by adding an email, password, and password_confirmation in seeds.rb:</p>
<pre><code class="lang-rb"><span class="setting">user = <span class="value">User.create(email: 'name@name.com', password: 'password1', password_confirmation: 'password1')</span></span>
</code></pre>
</div>
</div>
<span><span>Instructions</span></span>
<div class="accordion-section-body">
<div class="narrative--markdown">
<div class="fcn-checkpoint"><strong class="fcn-checkpoint__number"><span>1</span><span>.</span>
</strong>In seeds.rb, on line 9, add seed data for an <code>email</code>, <code>password</code>, and
<code>password_confirmation</code>. <br>
The password and password_confirmation must match. Hit Run.<strong class="fcn-checkpoint__number"><span></span></strong></div>
</div>
</div>
在 ** <span>db/seeds.rb</span> 文件里，加入：<br>
<pre><code class="lang-rb"><span class="setting">user = <span class="value">User.create(email: 'name@name.com', password: 'password1', password_confirmation: 'password1')</span></span></code></pre>
<p>即添加了一个user.</p>
<p><strong class="fcn-checkpoint__number"><span>2</span><span>.</span> </strong>In your terminal, run
<span style="font-family:Courier New">rake db:seed</span> to seed your database. Press Enter.(实际是： bundle exec rake db:seed)<pre code_snippet_id="671999" snippet_file_name="blog_20150521_8_5461082"  name="code" class="plain">$ bundle exec rake db:seed
$ </pre><br>
</p>
<h3>6. Complete the TopNav view for costumers</h3>
<p></p>
<div class="accordion-section-body">
<div class="accordion-section-body__padded narrative--markdown">
<p>Since our Users will be logging in through the home page we created earlier, we don't need to create separate views.<br>
我们不需要再给用户建立一个登录的view，因为我们首页已经有了。如果不是这样的话，我们要给用户一个区分，看是不是已经登进去了：</p>
i. If a User is signed in, we want to show the Sign Out link.
<p>ii. If a User is signed out, we want to show links that allow users to Sign In or Sign Up.</p>
<p>我们就用if ... else ... 语句，在view里，来完成这个事儿。<br>
</p>
<div class="fcn-checkpoint fcn-checkpoint--satisfied"><strong class="fcn-checkpoint__number"><span>1</span><span>.</span></strong>Use an if statement to see if a user is signed in with their current user email. To do this line 27, check
<code>if user_signed_in?</code> On line 28, type <code>current_user.email</code>.
<strong class="fcn-checkpoint__number"><span></span></strong></div>
<strong class="fcn-checkpoint__number"><span>2</span><span>.</span></strong>Use an else statement to display the sign in or sign up options. To do this add an
<code>else</code> statement on line 31, and an <code>end</code> statement on line 34.<strong class="fcn-checkpoint__number"><span></span></strong><br>
<p></p>
</div>
</div>
<p></p>
<p>file **<span> app/views/shared/_topnav.html.erb:</span></p>
<p><span></span><pre code_snippet_id="671999" snippet_file_name="blog_20150521_9_3382084"  name="code" class="html">&lt;!--=== Top ===--&gt;
&lt;div class=&quot;browse&quot;&gt;
  &lt;div class=&quot;container&quot;&gt;
    &lt;ul&gt;
      &lt;li&gt;Art&lt;/li&gt;
      &lt;li&gt;Home &amp; Living&lt;/li&gt;
      &lt;li&gt;Jewelry&lt;/li&gt;
      &lt;li&gt;Books &amp; Music&lt;/li&gt;
      &lt;li&gt;Women&lt;/li&gt;
      &lt;li&gt;Men&lt;/li&gt;
      &lt;li&gt;Kids&lt;/li&gt;
      &lt;li&gt;Vintage&lt;/li&gt;
      &lt;li&gt;Weddings&lt;/li&gt;
      &lt;li&gt;Crafts&lt;/li&gt;
    &lt;/ul&gt;
  &lt;/div&gt;
&lt;/div&gt;

&lt;div class=&quot;top&quot;&gt;
    &lt;div class=&quot;container&quot;&gt;
    &lt;div class=&quot;logo&quot;&gt;
      &lt;img src=&quot;https://www.etsy.com/assets/dist/images/etsylogo.20140703190113.png&quot;&gt;
    &lt;/div&gt;
    &lt;div&gt;
        &lt;ul class=&quot;header-nav&quot;&gt;
            &lt;% if user_signed_in? %&gt;&lt;!--# add your if statement here %&gt;--&gt;
              Logged in as &lt;strong&gt;
                &lt;%= current_user.email%&gt; &lt;!--# print out user email %&gt;--&gt;
              &lt;/strong&gt;.
              &lt;%= link_to &quot;Sign out&quot;, destroy_user_session_path, method: :delete, :class =&gt; &quot;btn btn-default&quot; %&gt;
            &lt;% else %&gt; &lt;!--#complete this %&gt;--&gt;
              &lt;%= link_to &quot;Sign up&quot;, new_user_registration_path, :class =&gt; &quot;btn btn-default&quot; %&gt;
              &lt;%= link_to &quot;Sign in&quot;, new_user_session_path, :class =&gt; &quot;btn btn-default&quot; %&gt;
            &lt;% end %&gt; &lt;!--#complete this %&gt;--&gt;
            &lt;li class=&quot;account&quot;&gt;
              &lt;div class=&quot;cart pull-right&quot;&gt;
                &lt;div class=&quot;fa fa-shopping-cart fa-2x&quot;&gt;
                &lt;/div&gt;
              &lt;/div&gt;
          &lt;/li&gt;
        &lt;/ul&gt;
    &lt;/div&gt;
&lt;/div&gt;
</pre><br>
===================</p>
<h3><span>7. 大结局<br>
</span></h3>
<p><span>We're finally ready to try out our authentication system! In your browser try to sign in with your email and the password you stored in your seeds file.</span><br>
In your browser, visit <strong><span style="font-family:Courier New">localhost:8000</span></strong> and sign in with your email and encrypted password.</p>
<p><span>但是我们这里得到了一个 <span style="color:#FF0000">No Method Error:<br>
</span><img src="http://img.blog.csdn.net/20150521144546406?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br>
</span></p>
<p><br>
</p>
<p>一个错误的结局，就是下一步努力的开始。Yeah!但是我还是得到了Codecademy.com颁发的荣誉证书：</p>
<p>&nbsp; <img src="http://img.blog.csdn.net/20150521144823373?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" height="343" width="404" alt=""></p>
<p><br>
</p>
<p><br>
</p>
   

---
layout: post
title: 'Ruby学习笔记4: 动态web app的建立'
date: 2015-05-07 15:38:00 +0800
category: from_cnblogs
slug: p20150507153800
---


<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-family:Oxygen,sans-serif; line-height:22px; font-size:16px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
</p>
<h2>Ruby学习笔记4： 动态web app的建立</h2>
<p></p>
<p>We will first build the Categories page. This page contains topics like Art, Home &amp; Living, and Kids, so our users can browse through categories and find what they like.<br>
</p>
<p>Each Category in our site will need to store information about itself, such as a name and an image. Our Category Model is what stores information about our Categories. Remember that our Model manages the data in our app.</p>
<p>===============================<br>
1. Build Model<br>
2. Build Migration Table<br>
3. Add Data to Tables<br>
4. Generate Table Controller<br>
5. <br>
===============================</p>
<h3></h3>
<h3>1. Build our Model</h3>
<p>开始之前，我们要先建立Model用来存储每个Category的数据模型。</p>
<p></p>
<p>We can generate a Model with the following rails command:</p>
<pre><code>rails generate model Person
</code></pre>
<p>Here <code>Person</code> is an example name for our Model. Model names are always singular.</p>
<span>这里，我们建立一个叫做Category的Model：<br>
In your terminal, generate a model named <code>Category</code> using the rails command. Press Enter.</span><br>
<p></p>
<pre><code>rails generate model Category
</code></pre>
In terminal:<pre code_snippet_id="661983" snippet_file_name="blog_20150508_1_7184323"  code_snippet_id="661983" snippet_file_name="blog_20150508_1_7184323" name="code" class="plain">$ rails generate model Category
invoke  active_record
create    db/migrate/20141013165110_create_categories.rb
create    app/models/category.rb
invoke    test_unit
create      test/models/category_test.rb
create      test/fixtures/categories.yml
$ </pre><br>
<p></p>
<h3>2. Build our Migration table</h3>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-family:Oxygen,sans-serif; line-height:22px; font-size:16px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
</p>
<p></p>
<p>Our Model is fine for now. Let's prepare our Migration table. We do this in two steps:</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-family:Oxygen,sans-serif; line-height:22px; font-size:16px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
a. We add columns to our Category Migration table. These will define what information our database can accept.</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-family:Oxygen,sans-serif; line-height:22px; font-size:16px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
b. We type bundle exec bundle exec rake db:migrate in our terminal to migrate our database, or update its state.</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-family:Oxygen,sans-serif; line-height:22px; font-size:16px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
Let's look at an example. Say we had a Person Model, and we want it to have the columns name and age. In the Migration table, we add columns like this:</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-family:Oxygen,sans-serif; line-height:22px; font-size:16px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
<span style="background-color:rgb(204,204,204)">def change<br>
&nbsp; create_table :person do |t|<br>
&nbsp; &nbsp; t.string :name<br>
&nbsp; &nbsp; t.integer :age<br>
&nbsp; &nbsp; t.timestamps<br>
&nbsp; end<br>
end</span><br>
</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-family:Oxygen,sans-serif; line-height:22px; font-size:16px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
<span style="background-color:rgb(204,204,204)"><span style="font-family:Oxygen,sans-serif; color:#4a4a4c; font-size:16px; line-height:22px; background-color:rgb(241,242,243)">Above we add&nbsp;</span><code style="">t.string :name</code><span style="font-family:Oxygen,sans-serif; color:#4a4a4c; font-size:16px; line-height:22px; background-color:rgb(241,242,243)">&nbsp;and</span><code style="">t.integer
 :age</code><span style="font-family:Oxygen,sans-serif; color:#4a4a4c; font-size:16px; line-height:22px; background-color:rgb(241,242,243)">&nbsp;specifying the name of type string and the age of type integer. We run db:migrate to add a name and age column to our
 actual database.</span><br>
</span></p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-family:Oxygen,sans-serif; line-height:22px; font-size:16px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
<span style="background-color:rgb(204,204,204)"><span style="font-family:Oxygen,sans-serif; color:#4a4a4c; font-size:16px; line-height:22px; background-color:rgb(241,242,243)"></span></span></p>
<div class="accordion-section-heading" style="margin:0px; padding:10px; border-width:0px 0px 1px; border-bottom-style:solid; border-bottom-color:rgb(241,242,242); font-family:Oxygen,sans-serif; font-size:16px; vertical-align:baseline; word-break:break-word; height:40px; background-color:rgb(230,231,232); color:rgb(32,64,86)">
<span style="font-family:inherit; margin:0px; padding:0px; border:0px; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word">Instructions</span></div>
<div class="accordion-section-body" style="">
<div class="narrative--markdown" style="margin:0px; padding:0px; border:0px; font-family:inherit; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; font-size:1em; vertical-align:baseline; word-break:break-word; height:inherit; background-color:rgb(241,242,243)">
<div class="fcn-checkpoint fcn-checkpoint--satisfied" style="margin:0px; padding:30px; border-width:1px 0px; border-top-style:solid; border-bottom-style:solid; border-top-color:rgb(235,250,247); border-bottom-color:white; font-family:inherit; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word; position:relative; background-color:rgb(235,250,247)">
<span class="fcn-checkpoint__number" style="font-family:inherit; margin:0px; padding:0px; border:0px; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word; text-align:right; position:absolute; top:30px; left:50px; width:28px"><span style="font-family:inherit; margin:0px; padding:0px; border:0px; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word">1</span><span style="font-family:inherit; margin:0px; padding:0px; border:0px; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word">.</span></span>
<div class="fcn-checkpoint__body" style="margin:0px 0px 0px 55px; padding:0px; border:0px; font-family:inherit; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word">
<span style="font-family:inherit; margin:0px; padding:0px; border:0px; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word"></span>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; border:0px; font-family:inherit; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:22px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
Add a&nbsp;<code style="">name</code>&nbsp;and&nbsp;<code style="">thumburl</code>&nbsp;attributes to your category migrations. These are both strings. Hit Run.</p>
</div>
<div class="fcn-checkpoint__checkbox fcn-checkpoint__checkbox--satisfied" style="margin:0px; padding:0px; border:0px; font-family:inherit; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word; position:absolute; top:30px; left:30px; background-color:rgb(57,209,180); width:20px; height:20px; color:white">
<div class="fcn-icon fcn-icon-checkmark" style=""></div>
</div>
</div>
<div class="fcn-checkpoint fcn-checkpoint--satisfied" style="margin:0px; padding:30px; border-width:0px 0px 1px; border-bottom-style:solid; border-bottom-color:white; font-family:inherit; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word; position:relative; background-color:rgb(235,250,247)">
<span class="fcn-checkpoint__number" style="font-family:inherit; margin:0px; padding:0px; border:0px; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word; text-align:right; position:absolute; top:30px; left:50px; width:28px"><span style="font-family:inherit; margin:0px; padding:0px; border:0px; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word">2</span><span style="font-family:inherit; margin:0px; padding:0px; border:0px; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word">.</span></span>
<div class="fcn-checkpoint__body" style="margin:0px 0px 0px 55px; padding:0px; border:0px; font-family:inherit; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word">
<span style="font-family:inherit; margin:0px; padding:0px; border:0px; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word"></span>
<p style="margin-top:0px; margin-bottom:0px; padding-top:0px; padding-bottom:0px; border:0px; font-family:inherit; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:22px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
In your terminal, run<code style="">bundle exec rake db:migrate</code>&nbsp;to migrate your database. Press Enter.</p>
</div>
</div>
</div>
</div>
<pre code_snippet_id="661983" snippet_file_name="blog_20150508_2_5816976"  code_snippet_id="661983" snippet_file_name="blog_20150508_2_5816976" name="code" class="ruby">class CreateCategories &lt; ActiveRecord::Migration
  def change
    create_table :categories do |t|
			t.string :name
			t.string :thumburl
      #t.timestamps
    end
  end
end</pre><br>
<p>Terminal：</p>
<pre code_snippet_id="661983" snippet_file_name="blog_20150508_3_1766650"  code_snippet_id="661983" snippet_file_name="blog_20150508_3_1766650" name="code" class="plain">$ bundle exec rake db:migrate
== 20140626211003 CreateCategories: migrating =================================
-- create_table(:categories)
   -&gt; 0.0010s
== 20140626211003 CreateCategories: migrated (0.0010s) ========================

$ </pre><br>
<h3>3. Add data to Tables<br>
</h3>
<p>Now that we have columns, we need to add data. This file(in the <span style="font-family:Courier New">
seeds.rb</span> file) on the below might look intimidating, but it is a simple list of data for our Rails app.</p>
<pre code_snippet_id="661983" snippet_file_name="blog_20150508_4_1926820"  code_snippet_id="661983" snippet_file_name="blog_20150508_4_1926820" name="code" class="ruby"># This file should contain all the record creation needed to seed the database with its default values.
# The data can then be loaded with the rake db:seed (or created alongside the db with db:setup).
#
# Examples:
#
#   cities = City.create([{ name: &#39;Chicago&#39; }, { name: &#39;Copenhagen&#39; }])
#   Mayor.create(name: &#39;Emanuel&#39;, city: cities.first)

	art = Category.create(name: &#39;Art&#39;, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/e/eb/144-la_fuente_de_Monforte_V.jpg&#39;)
	home_and_living = Category.create(name: &#39;Home &amp; Living&#39;, thumburl: &#39;http://ihomedecorsideas.com/wp-content/uploads/2014/04/diy_network_homemade_coat_rack_.jpg&#39;)
	jewelry = Category.create(name: &#39;Jewelry&#39;, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/f/ff/Midyat_Silver_Jewelry_1310103_Nevit.jpg&#39;)
	women = Category.create(name: &#39;Women&#39;, thumburl: &#39;https://c1.staticflickr.com/9/8255/8660920433_57a184d9d1_z.jpg&#39;)
	men = Category.create(name: &#39;Men&#39;, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/d/d5/Fullbrogue_(Grenson).jpg&#39;)
	kids = Category.create(name: &#39;Kids&#39;, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/e/e0/Artist%27s_Paint_Brushes_Adapted_With_Photoshop._Surrey_UK.jpg&#39;)
	vintage = Category.create(name: &#39;Vintage&#39;, thumburl: &#39;https://c2.staticflickr.com/8/7402/9426557291_139134efaa_z.jpg&#39;)
	weddings = Category.create(name: &#39;Weddings&#39;, thumburl: &#39;http://hostingessence.com/wp-content/uploads/2012/04/green-wedding.jpg&#39;)
  # Add your category here
</pre>
<p>How does this work? We store data in two steps.</p>
<p>a. We first add seed data in the <strong><span style="font-family:Courier New">seeds.rb</span></strong> file</p>
<p>b. We run <strong><span style="font-family:Courier New">rake db:seed</span></strong> in our terminal.</p>
<p>已经加了数据在例子里，这里再练习一下seeding data : Let's look at a small example first.</p>
<pre><code class="lang-rb"><span class="setting">gia = <span class="value">Person.create(name: 'Gia', age: '<span class="number">8</span>')</span></span>
</code></pre>
<p>Here we create a new object in the Person class with the name <strong>Gia</strong> and age<strong>8</strong>. We store this object in a variable<strong>gia</strong>.</p>
<p>Now let's look at some seed data for Category.</p>
<pre><code class="lang-rb"><span class="setting">weddings = <span class="value">Category.create(name: 'Weddings', thumburl: 'http://hostingessence.com/wp-content/uploads/<span class="number">2012</span>/<span class="number">04</span>/green-wedding.jpg')</span></span>
</code></pre>
<p>Here, we create a new object in the Category class with the <strong>name</strong> of Weddings and a<strong>thumburl</strong>. We store this in the variable weddings.</p>
<p>=========== Instructions ===========</p>
<p></p>
<div class="fcn-checkpoint fcn-checkpoint--satisfied"><strong class="fcn-checkpoint__number"><span>&lt;1&gt;</span><span>.</span></strong>
<div class="fcn-checkpoint__body"><span></span>
<p>Look in your seeds file, seeds.rb. On line 17, add seed data for <code>craft_supplies</code>. The entry should have a name<code>'Craft Supplies'</code> and a thumburl of<code>'http://bit.ly/1w1uPow'</code>. Store this entry in a variable called craft_supplies.
 Hit Run.</p>
</div>
</div>
<strong class="fcn-checkpoint__number"><span>&lt;2&gt;</span><span>.</span></strong>
<div class="fcn-checkpoint__body"><span></span>
<p>In your terminal, run <code>rake db:seed</code> [更正：<span style="background-color:rgb(204,204,204)"><span style="font-family:Courier New">bundle exec rake db:seed<span style="background-color:rgb(204,204,204)"></span></span></span> ] to add the data from
 seeds into your database. Press Enter.</p>
</div>
in seeds.db file, add below at line 17:<pre code_snippet_id="661983" snippet_file_name="blog_20150508_5_6574792"  code_snippet_id="661983" snippet_file_name="blog_20150508_5_6574792" name="code" class="ruby">  # Add your category here:
craft_supplies = Category.create(name:&#39;Craft Supplies&#39;,thumburl:&#39;http://bit.ly/1w1uPow&#39;)</pre>in Terminal, type this command and hit Enter:<pre code_snippet_id="661983" snippet_file_name="blog_20150508_6_8686810"  code_snippet_id="661983" snippet_file_name="blog_20150508_6_8686810" name="code" class="plain">$ rake db:seed
rake aborted!
Gem::LoadError: You have already activated rake 10.4.2, but your Gemfile requires rake 10.3.2. Prepending `bundle exec` to your command may solve this.
/var/lib/gems/2.0.0/gems/bundler-1.7.3/lib/bundler/runtime.rb:34:in `block in setup&#39;
/var/lib/gems/2.0.0/gems/bundler-1.7.3/lib/bundler/runtime.rb:19:in `setup&#39;
/var/lib/gems/2.0.0/gems/bundler-1.7.3/lib/bundler.rb:121:in `setup&#39;
/var/lib/gems/2.0.0/gems/bundler-1.7.3/lib/bundler/setup.rb:17:in `&lt;top (required)&gt;&#39;
/home/ccuser/workspace/ecommerce-rails-app/config/boot.rb:4:in `&lt;top (required)&gt;&#39;
/home/ccuser/workspace/ecommerce-rails-app/config/application.rb:1:in `&lt;top (required)&gt;&#39;
/home/ccuser/workspace/ecommerce-rails-app/Rakefile:4:in `&lt;top (required)&gt;&#39;
(See full trace by running task with --trace)
$ </pre>我们得到一个错误：rake aborted! 看它的说明，使用这个命令即可：<pre code_snippet_id="661983" snippet_file_name="blog_20150512_7_5715260"  code_snippet_id="661983" snippet_file_name="blog_20150512_7_5715260" name="code" class="plain">$ bundle exec rake db:seed</pre><br>
OK.&nbsp;
<h3>4.Generate table Controller<br>
</h3>
建立了数据库操作的基本东东，但目前我们table里的数据是static的，所以我们又要建立Controller，让我们来改变页面里的内容。
<p>但目前我们table里的数据是static的，所以我们又要建立Controller，让我们来改变页面里的内容。In Rails，数据库操作的五个主要的方式methods： index, show, new, edit, delete. 这几个在动态Rails App里是一样的，对以后的学习有帮助。<br>
还记得我们建立Persons Controller时候用的命令吗？<br>
&gt;&gt;&nbsp; <span style="font-family:Courier New"><span style="background-color:rgb(204,204,204)">rails genertate controller Persons</span></span><br>
所以我们就可以在命令行里添加methods，同时，Rails会updates相关的routes和Views for us.<br>
例如，如果我们想要添加 index ，new, edit 这三个mothods到Persons这个Controller，使用如下的命令：<br>
&gt;&gt;&nbsp; <span style="font-family:Courier New"><span style="background-color:rgb(204,204,204)">rails generate controller Persons index new edit</span></span><br>
注：一般Controller的名字是plural复数，Model的名字是单数。<br>
【好，这里的操作】1.In your terminal, generate a controller Categories with the main methods we’ll use: index, show, new, edit, and delete. Press Enter.<br>
&gt;&gt;&nbsp;<span style="background-color:rgb(204,204,204)"><span style="font-family:Courier New"> rails generate controller Categories index show new edit delete</span></span><br>
</p>
<pre code_snippet_id="661983" snippet_file_name="blog_20150511_7_7905412"  code_snippet_id="661983" snippet_file_name="blog_20150511_7_7905412" name="code" class="plain">$ rails generate controller Categories index show new edit delete
create  app/controllers/categories_controller.rb
 route  get &#39;categories/delete&#39;
 route  get &#39;categories/edit&#39;
 route  get &#39;categories/new&#39;
 route  get &#39;categories/show&#39;
 route  get &#39;categories/index&#39;
invoke  erb
create    app/views/categories
create    app/views/categories/index.html.erb
create    app/views/categories/show.html.erb
create    app/views/categories/new.html.erb
create    app/views/categories/edit.html.erb
create    app/views/categories/delete.html.erb
invoke  test_unit
create    test/controllers/categories_controller_test.rb
invoke  helper
create    app/helpers/categories_helper.rb
invoke    test_unit
create      test/helpers/categories_helper_test.rb
invoke  assets
invoke    coffee
create      app/assets/javascripts/categories.js.coffee
invoke    scss
create      app/assets/stylesheets/categories.css.scss
$ </pre><br>
done!这样我们就又建立了一个Controller，里面有操作数据库的5个methods,如下：<br>
catogories_controller.rb文件：<pre code_snippet_id="661983" snippet_file_name="blog_20150511_8_1889700"  code_snippet_id="661983" snippet_file_name="blog_20150511_8_1889700" name="code" class="ruby">class CategoriesController &lt; ApplicationController
  def index
  end

  def show
  end

  def new
  end

  def edit
  end

  def delete
  end
end</pre>然后我们就要来写这些methods里的具体的语句。<br>
<p></p>
<h4>---&lt;1&gt;---关于安全</h4>
<p>但是，开始之前，我们要保证从model来的数据的安全，所以：<br>
There's one step we do before writing our main Controller methods: we need our Controller to have a secure way of getting information from our Model.<br>
We'll <strong>use a private method to do this</strong> called <span style="font-family:Courier New">
strong parameters</span>. This method prevents a user from hacking into our app and changing our Model.<br>
要让method ”strong parameters“ 变得private从而阻止黑客入侵。首先前面有个private, 然后加上这个strong params method, 这个会store all our infor 并且只让开发者访问下面的method.e.g.:</p>
<pre code_snippet_id="661983" snippet_file_name="blog_20150511_9_3780435"  code_snippet_id="661983" snippet_file_name="blog_20150511_9_3780435" name="code" class="ruby">private
def person_params
  params.require(:person).permit(:name, :age)
end
</pre>这里我们是以persons Controller为例，we make a private person_params method. In the body of the method, we require the Model name (person), and permit the columns (name, age).现在任何时候我们想要person的信息，我们都引用为我们存储了信息的person_params method.<br>
<p></p>
<p>【好，这里的操作】<span>In categories_controller.rb, beneath the <code>end</code> after<code>def delete</code>, declare a<code>private</code> method. Under the private, create a<code>category_params</code> method for categories. Remember to require<code>category</code>
 and permit a<code>name</code> and <code>thumburl</code>. Use<code>end</code> to close it. Hit Run.</span><br>
</p>
<pre code_snippet_id="661983" snippet_file_name="blog_20150511_10_6750966"  code_snippet_id="661983" snippet_file_name="blog_20150511_10_6750966" name="code" class="ruby">class CategoriesController &lt; ApplicationController
  def index
  end

  def show
  end

  def new
  end

  def edit
  end

  def delete
  end
  
  #declare a private method
  private 
  def category_params
  	params.require(:category).permit(:name, :thumburl)
  end
end
</pre>上面用到了params.require，是一个验证method, 大意是只有name 和 thumburl可以被验证通过，存入model，详见：http://stackoverflow.com/questions/18424671/what-is-params-requireperson-permitname-age-doing-in-rails-4<br>
<p></p>
<h4>---&lt;2&gt;---加入methods内容</h4>
我们来丰富Controller的methods内容：<br>
<p></p>
<p>**First, the index method. The index method takes every entry in our database and stores it as a variable.</p>
<p>If each record in our app is about person, the index method would show us all <strong>
persons</strong> in our database. Just like a real index. It looks like this:</p>
<pre><code class="lang-rb"><span class="function"><span class="keyword">def</span> <span class="title"><span class="keymethods">index</span></span></span>
 <span class="variable">@persons</span> = <span class="constant">Person</span>.<span class="identifier">all</span>
<span class="identifier"><span class="keyword">end</span></span>
</code></pre>
<p>Here we take all persons, and store them in an instance variable @persons.<br>
【操作】<br>
在c<span>ategories_controller.rb</span>文件中：<br>
</p>
<pre code_snippet_id="661983" snippet_file_name="blog_20150511_11_8226998"  code_snippet_id="661983" snippet_file_name="blog_20150511_11_8226998" name="code" class="ruby">  def index
  	@categories = Category.all
  end</pre>开始丰富下一个method之前，我们先来试一下index工作没有。<br>
<br>
=====&nbsp; 接下来就不知所云&nbsp; =====<br>
<br>
在Rails App的Request-Response Cycle里，我们已知一个url请求来了要经过route分发到正确的Controller &amp; View. 对于五个数据库操作的methods，我们都要有route and view 与之对应。<br>
<br>
&nbsp;Recall how we said Rails does some magic for us. It actually does two pieces of magic when we generate a Controller with methods!<br>
a. It updates the routes file routes.rb with resources :categories. This shorthand will take care of our routes for the methods we will implement in Categories Controller.<br>
b. It creates index.html.erb in the Views folder<br>
When we generate a Controller with a given methods, it creates a route and View for us, magically. Otherwise, we have to do this manually.<br>
<br>
【Instructions】<br>
Check out routes.rb to see resources :categories. Then, in the browser window, visit localhost:8000/categories to see the index view Rails created.<br>
**在<span></span><span>config/routes.rb</span>文件中，<pre code_snippet_id="661983" snippet_file_name="blog_20150512_12_7209561"  code_snippet_id="661983" snippet_file_name="blog_20150512_12_7209561" name="code" class="ruby">Rails.application.routes.draw do
  get &#39;/&#39; =&gt; &#39;pages#home&#39;   #这样写总算是让人有点明白了

  resources :categories     #这是一种方法，还有以下两种方法：
&#160; #get &#39;/categories&#39; =&gt; &#39;categories#index&#39;
&#160; #get &#39;categories/index&#39;   #browser_url: http://localhost:8000/categories/index
  
  get &#39;categories/:id/delete&#39; =&gt; &#39;categories#delete&#39;, :as =&gt; :categories_delete
end</pre><br>
**在index.html.erb文件中:<pre code_snippet_id="661983" snippet_file_name="blog_20150512_13_6492982"  code_snippet_id="661983" snippet_file_name="blog_20150512_13_6492982" name="code" class="html">&lt;h1&gt;Categories#index&lt;/h1&gt;
&lt;p&gt;Find me in app/views/categories/index.html.erb&lt;/p&gt;</pre>
<p><br>
</p>
<p>Rails建立了一个route and View for us, 但是是个空的文件，改变index.html.erb文件，让它看起来好点。index视图应该怎么样的？index method保存了数据库的所有条目，所以index 视图应该显示出所有条目。看个例子：<br>
Say we have a persons index page want to show all persons in our collection.<br>
We write:<br>
--------------<br>
<span style="font-family:Courier New">&lt;% @persons.each do |person| %&gt;<br>
&lt;tr&gt;<br>
&nbsp; &lt;td&gt;&lt;%= person.name %&gt;&lt;/td&gt;<br>
&nbsp; &lt;td&gt;&lt;%= person.age %&gt;&lt;/td&gt;<br>
&lt;/tr&gt;<br>
&lt;% end %&gt;</span><br>
--------------<br>
Here we go through each person stored in @persons with an iterator and print out the attributes person.name and person.age.<br>
然后把这个应用在index.html.erb的cagegory组里：<br>
**index.html.erb文件：</p>
<pre code_snippet_id="661983" snippet_file_name="blog_20150512_15_8625924"  code_snippet_id="661983" snippet_file_name="blog_20150512_15_8625924" name="code" class="html">&lt;h1&gt;Listing categories&lt;/h1&gt;

&lt;table&gt;
&#160; &lt;thead&gt;
&#160;&#160;&#160; &lt;tr&gt;
&#160;&#160;&#160;&#160;&#160; &lt;th&gt;Name&lt;/th&gt;
&#160;&#160;&#160;&#160;&#160; &lt;th&gt;Thumburl&lt;/th&gt;
&#160;&#160;&#160;&#160;&#160; &lt;th colspan=&quot;3&quot;&gt;&lt;/th&gt;
&#160;&#160;&#160; &lt;/tr&gt;
&#160; &lt;/thead&gt;

&#160; &lt;tbody&gt;
&#160;&#160;&#160; &lt;% @categories.each do |category| %&gt;
&#160;&#160;&#160;&#160;&#160; &lt;tr&gt;
&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;td&gt;&lt;%= category.name %&gt;&lt;/td&gt;
&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;td&gt;&lt;%= category.thumburl %&gt;&lt;/td&gt;
&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;td&gt;&lt;%= link_to &#39;Show&#39;, category_path(category) %&gt;&lt;/td&gt;
&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;td&gt;&lt;%= link_to &#39;Edit&#39;, edit_category_path(category) %&gt;&lt;/td&gt;
&#160;&#160;&#160;&#160;&#160;&#160;&#160; &lt;td&gt;&lt;%= link_to &#39;Destroy&#39;, category, method: :delete, data: { confirm: &#39;Are you sure?&#39; } %&gt;&lt;/td&gt;
&#160;&#160;&#160;&#160;&#160; &lt;/tr&gt;
&#160;&#160;&#160; &lt;% end %&gt;
&#160; &lt;/tbody&gt;
&lt;/table&gt;

&lt;br&gt;

&lt;%= link_to &#39;New Category&#39;, new_category_path %&gt;
</pre>
<p></p>
<p>此时再来访问localhost:8000/categories即可获得列表：</p>
<p><img src="http://img.blog.csdn.net/20150512144630559?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br>
</p>
<p>The index page we made has all the right information, but it didn't have the structure and style. We've added the supporting HTML code for the view.<br>
文件和信息流程图如下，<img src="http://img.blog.csdn.net/20150513113445870?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br>
但有几个疑问：<span style="color:#FF0000"><br>
<span style="color:#000000">1. config/routes.rb文件中</span>get 'categories/:id/delete' =&gt; 'categories#delete', :as =&gt; :categories_delete</span>不能动，为啥？<br>
2. routes.rb 与 Controller是怎么联系的？或者routes.rb, controller, view 三者是怎么联系？<br>
3. index.html.erb文件中，<br>
<br>
以下是加入了style效果的html：</p>
<pre code_snippet_id="661983" snippet_file_name="blog_20150512_16_4038671"  code_snippet_id="661983" snippet_file_name="blog_20150512_16_4038671" name="code" class="html">&lt;%= render &#39;shared/topnav&#39; %&gt;

&lt;div class=&quot;categories&quot;&gt;
      &lt;div class=&quot;container&quot;&gt;
        &lt;div class=&quot;row&quot;&gt;
          &lt;div class=&quot;col-md-2&quot;&gt;
            &lt;h2&gt;Categories&lt;/h2&gt;
            &lt;p&gt;Explore our latest categories from around the world.&lt;/p&gt;
          &lt;/div&gt;
          &lt;div class=&quot;col-md-8&quot;&gt;
            &lt;% @categories.in_groups_of(3).each do |categories| %&gt;
              &lt;% categories.select! {|x| !x.nil?} %&gt;
              &lt;div class=&#39;row&#39;&gt;
                &lt;% categories.each do |category| %&gt;
                  &lt;div class=&#39;col-md-4&#39;&gt;
                    &lt;div class=&quot;thumbnail&quot;&gt;
                        &lt;img src= &lt;%= category.thumburl %&gt; &gt;
                        &lt;div class=&quot;caption&quot;&gt;
                          &lt;span class=&quot;listing-title&quot;&gt;&lt;%= category.name %&gt;&lt;/span&gt;
                          &lt;span&gt;&lt;%= link_to &quot;Edit&quot;, edit_category_path(category.id) %&gt;&lt;/span&gt;
                          &lt;span&gt;&lt;%= link_to &#39;Show&#39;, category %&gt;&lt;/span&gt;
                          &lt;td&gt;&lt;%= link_to &#39;Delete&#39;, categories_delete_path(:id =&gt; category.id) %&gt;&lt;/td&gt;
                        &lt;/div&gt;
                      &lt;/div&gt;
                    &lt;/div&gt;
                &lt;% end %&gt;
              &lt;/div&gt;
            &lt;% end %&gt;

          &lt;/div&gt;
        &lt;/div&gt;

      &lt;/div&gt;
    &lt;/div&gt;

&lt;table&gt;
  &lt;thead&gt;
    &lt;tr&gt;
      &lt;th&gt;Category name&lt;/th&gt;
      &lt;th colspan=&quot;3&quot;&gt;&lt;/th&gt;
    &lt;/tr&gt;
  &lt;/thead&gt;

  &lt;tbody&gt;
    &lt;% @categories.each do |category| %&gt;
      &lt;tr&gt;
        &lt;td&gt;&lt;%= category.name %&gt;&lt;/td&gt;
        &lt;div class=&quot;thumbnail&quot;&gt;
           &lt;img src= &lt;%=category.thumburl %&gt; &gt;
        &lt;div&gt;
        &lt;td&gt;&lt;%= link_to &#39;Show&#39;, category %&gt;&lt;/td&gt;
        &lt;td&gt;&lt;%= link_to &#39;Edit&#39;, edit_category_path(category) %&gt;&lt;/td&gt;
        &lt;td&gt;&lt;%= link_to &#39;Delete&#39;, categories_delete_path(:id =&gt; category.id) %&gt;&lt;/td&gt;
      &lt;/tr&gt;
    &lt;% end %&gt;
  &lt;/tbody&gt;
&lt;/table&gt;
&lt;br&gt;

&lt;%= link_to &#39;New Category&#39;, new_category_path %&gt;

&lt;%= render &#39;shared/footer&#39; %&gt;
</pre><img src="http://img.blog.csdn.net/20150512150144115?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" height="278" width="586"><br>
验证了。没搞懂。
<p></p>
<p>**<span> </span><span>app/controllers/categories_controller.rb</span>文件：</p>
<pre code_snippet_id="661983" snippet_file_name="blog_20150512_17_4991315"  code_snippet_id="661983" snippet_file_name="blog_20150512_17_4991315" name="code" class="ruby">class CategoriesController &lt; ApplicationController
  def index
    @categories = Category.all
  end

  def show
  end

  def new
  end

  def edit
  end

  def delete
  end
  
  private
 	def category_params
  	params.require(:category).permit(:name, :thumburl)
  end
  	
end
</pre><br>
<br>
<p></p>
================<br>
<p>继续丰富我们的controller:<br>
While index gave all categories, show allows us to access one category. This this is helpful when we want to show just one Category at a time in our Etsy app.</p>
<p>The show method works like this. we write:</p>
<pre code_snippet_id="661983" snippet_file_name="blog_20150512_18_2912317"  code_snippet_id="661983" snippet_file_name="blog_20150512_18_2912317" name="code" class="ruby">def show
  @category = Category.find(params[:id])
end
</pre><br>
<p></p>
<br>
<p><br>
</p>
<p><br>
</p>
<p></p>
<h3>5. <br>
</h3>
<p><br>
</p>
<p><br>
</p>
<p style="margin-top:0px; margin-bottom:1em; padding-top:0px; padding-bottom:0px; border:0px; font-family:Oxygen,sans-serif; line-height:22px; font-size:16px; vertical-align:baseline; word-break:break-word; color:rgb(74,74,76)!important">
<span style="background-color:rgb(204,204,204)"><br>
</span></p>
   

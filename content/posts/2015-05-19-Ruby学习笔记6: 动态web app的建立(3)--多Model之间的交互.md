---
layout: post
title: 'Ruby学习笔记6: 动态web app的建立(3)--多Model之间的交互'
date: 2015-05-19 06:49:00 +0800
category: from_cnblogs
slug: p20150519064900
---


<div class="accordion-section-body">
<div class="accordion-section-body__padded narrative--markdown">
<p>We first built a static site which displayed a static image using only a Controller and a View. This is our Etsy landing page page.</p>
<p>Then we built the Categories page, with a Model (manages data), Controller (manages decisions) and View (manages display). The Categories pages allows us to display dynamic content so users can browse through different Categories that can be updated regularly.</p>
<p>Now we will build the Products page, which will allow users to browse through Products in our Etsy app. To create this, we'll be creating a new Model, Controller, and View.</p>
<p>Now, because have more than one Model, we will also learn how Models interact with each other through associations.</p>
<p>前面我们完成的ruby on rails app 的静态、动态网页开发，实现了数据库增删改查的操作，熟悉了MVC结构的工作机理。本文在前面的基础上，要新增一个Prodect的Model,从而体现多model交互的功能。<br>
<br>
</p>
<p>====================================</p>
<p>Product Page 的效果如下：<br>
<img src="http://img.blog.csdn.net/20150519145837880?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" height="428" width="769"><br>
</p>
<p><br>
</p>
<div class="accordion-section-body">
<div class="accordion-section-body__padded narrative--markdown">
<p>Let's review our Request-Response Cycle one more time.</p>
<p>Again, in our Products page, we're using a database just like we did for Categories.</p>
<ul>
<li><span style="background-color:rgb(255,255,0)">Our browser will first make a request to the server. The server handles the requests by talking to our Rails application.</span></li><li><span style="background-color:rgb(255,255,0)">Second, the route takes the request and finds the right Controller and method to handle the request.</span></li><li><span style="background-color:rgb(255,255,0)">Third, the Controller gathers data directly from our Model. It assembles the data in methods and then passes it on the View.</span></li><li><span style="background-color:rgb(255,255,0)">Finally, the View packages the information and sends it back to our browser.</span></li></ul>
</div>
</div>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="http://img.blog.csdn.net/20150519150112667?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" height="327" width="558">
<p></p>
<h3>Request-Response Cycle的工作机理：</h3>
<p>1. 浏览器发送请求到服务器，服务器处理请求，怎么处理：跟Rails App 说：有个请求来了！<br>
2. the routes 拿到请求，找到正确的Controller和Method，来处理这个请求。<br>
3. Controller 开始收集数据，从Model里收集，在Methods里组装数据，完了就传递给View<br>
4. 最后，View把所有的信息打包好，包括数据，样式，文本，图片各种。。。传回给浏览器。<br>
<br>
========================================<br>
</p>
<h2>下面开始我们Making Product Page<br>
</h2>
<p>This page will list all products that users buy - like shoes or craft paper. Users will be able to browse through products and find what they like.<br>
When we created a Model for Categories, we were able to add information about our categories. For Products, we will also create a Model so we can add information about our products, such as a product name or price.</p>
<h3>1. Generate the product model</h3>
<p>we generate a model like this:(in terminal)</p>
<pre><code> rails generate model product
</code></pre>
<p></p>
<p>Remember that Model names are always singular.<br>
When we generate a Product Model, we create corresponding Model and Migration files. Let's first focus on the product model.<br>
</p>
<h3>2. build the association between two models</h3>
<p>Since we have more than one Model, a Category Model and a Product Model, we can build the association between the two.<br>
Why do we need associations? As you create the Etsy app, you want to make sure that certain Products are associated with a Category. What if we want to put our painting in the Art category, or a toy in the Kids category?<br>
Associations do just this. They tell us how parts of our app relate to other parts.Association（关联）就是解决我们app的一些parts和另一些parts 是怎么个关系。<br>
How do we create associations? The most basic type is a has_many and belongs_to relationship, where 'one thing' has many 'other things'. Imagine a Person and their Pets. The Person has_many Pets, and Pets belongs_to the Person.怎么来创建一个关联呢？<br>
最基本的类型就是 has_many 和 belongs_to 关系。has_many 就是 ‘一个东西’ 有很多 ‘其他东西们’ ，设想，一个人和他的宠物们。就是这个人has_many 宠物们，并且，宠物们 belongs_to 这个人。<br>
</p>
<p>In the Person Model we type:</p>
<pre><code class="lang-rb"><span class="class"><span class="keyword">class</span> <span class="title">Person</span>
  <span class="title">has_many</span> :</span>pets
end
</code></pre>
<p>In the Pet Model, we type:</p>
<pre><code class="lang-rb"><span class="class"><span class="keyword">class</span> <span class="title">Pets</span>
  <span class="title">belongs_to</span> :</span>person
end
</code></pre>
<p><br>
</p>
<p>In our case, our Category has many products, and Product belongs to category:</p>
<pre><code class="lang-rb"><span class="class"><span class="keyword">class</span> <span class="title">Category</span>
  <span class="title">has_many</span> :</span>products
end
</code></pre>
<p>In the Pet Model, we type:</p>
<pre><code class="lang-rb"><span class="class"><span class="keyword">class</span> <span class="title">Products</span>
  <span class="title">belongs_to</span> :</span>category
end
</code></pre>
**<span></span><span>app/models/category.rb</span>:<pre code_snippet_id="670722" snippet_file_name="blog_20150519_1_7742902"  code_snippet_id="670722" snippet_file_name="blog_20150519_1_7742902" name="code" class="ruby">class Category &lt; ActiveRecord::Base
	has_many :products
end
</pre>同时，<br>
**<span>app/models/product.rb</span>:<pre code_snippet_id="670722" snippet_file_name="blog_20150519_2_3254708"  code_snippet_id="670722" snippet_file_name="blog_20150519_2_3254708" name="code" class="ruby">class Product &lt; ActiveRecord::Base
	belongs_to :category
end</pre><br>
<h3>3. prepare our Products Migration table</h3>
<p></p>
<p></p>
<p>Now that our Product Model is set up, we need to prepare our Products Migration table so that it has the right columns.</p>
<p>If you recall from the Categories Migration, we do this in two steps:</p>
<p>a. Add columns to our Products Migration table</p>
<p>b. Type <span style="font-family:Courier New">bundle exec rake db:migrate</span> in the terminal</p>
<p>In this case, there's one more column that we will add. Now that we have an association, we need to tell our Models that our Product Model belongs to our Category Model by adding a<strong>references</strong> column.</p>
we had a Product that belonged to a Category, we would add<span style="font-family:Courier New"> t.references</span>to our<span style="font-family:Courier New">Pet</span> table to look like this:
<p><br>
</p>
<p>in our Products Migration table, we will <code>:references</code> to Category. We always add references to the table that<code>belongs_to</code> something else.[从model里添加references to 主model.] 这里我们就是产品model的文件里，Reference to Category, buz product model belongs
 to category model.</p>
<p>1. 文件**db/migrate/20140626211017_create_products.rb: </p>
<pre code_snippet_id="670722" snippet_file_name="blog_20150519_3_4174333"  code_snippet_id="670722" snippet_file_name="blog_20150519_3_4174333" name="code" class="ruby">class CreateProducts &lt; ActiveRecord::Migration
&#160; def change
&#160;&#160;&#160; create_table :products do |t|
&#160;&#160; &#160;&#160;&#160; &#160;t.string :product_name&#160; #string
&#160;&#160; &#160;&#160;&#160; &#160;t.text :description&#160;&#160;&#160;&#160; #text
&#160;&#160; &#160;&#160;&#160; &#160;t.float :price&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; #float
&#160;&#160; &#160;&#160;&#160; &#160;t.string :thumburl&#160;&#160;&#160;&#160;&#160; #string
&#160;&#160; &#160;&#160;&#160; &#160;t.references :category&#160; #references
&#160;&#160; &#160;&#160;&#160; &#160;t.timestamps&#160;&#160; &#160;&#160;&#160; &#160;    #timestamps is required.
&#160;&#160;&#160; end
&#160; end
end</pre>
<p></p>
<p><span>2. In terminal, run <span style="background-color:rgb(204,204,204)"><code>bundle exec rake db:migrate</code></span> to migrate your database. Press Enter.</span><br>
</p>
<br>
<pre code_snippet_id="670722" snippet_file_name="blog_20150519_4_9063691"  code_snippet_id="670722" snippet_file_name="blog_20150519_4_9063691" name="code" class="plain">$ bundle exec rake db:migrate
== 20140626211017 CreateProducts: migrating ===================================
-- create_table(:products)
   -&gt; 0.0013s
== 20140626211017 CreateProducts: migrated (0.0013s) ==========================

$ </pre><br>
<p></p>
<h3>4. Add seed data for our Products</h3>
<p></p>
<p></p>
<p>Now that we have columns, let's add seed data for our Products. If you recall there are also two steps for this:</p>
<p><span style="font-family:Courier New">a. We add seed data in our seeds.rb file</span></p>
<p><span style="font-family:Courier New">b. We run <span style="background-color:rgb(204,204,204)">
rake db:seed</span> in the terminal (<span style="background-color:rgb(204,204,204)"><span style="font-family:Courier New">bundle exec rake db:seed<span style="background-color:rgb(204,204,204)"></span></span></span>)</span></p>
<p>We already added most of the Products seed data for you. This time with one small change. Since each product belongs to a specific category, we'll also be adding a category_id at the end for art.</p>
<p>Let's look at some seed data:</p>
<pre><code class="lang-rb"><span class="constant">Product</span>.<span class="identifier">create</span>(<span class="identifier">product_name</span><span class="symbol">:</span> <span class="string">'Spanish Canvas Painting'</span>, <span class="identifier">description</span><span class="symbol">:</span> <span class="string">'La Fuente de Monteforte Painting Acrylic'</span>, <span class="identifier">price</span><span class="symbol">:</span> <span class="number">79.00</span>, <span class="identifier">thumburl</span><span class="symbol">:</span> <span class="string">'http://upload.wikimedia.org/wikipedia/commons/e/eb/144-la_fuente_de_Monforte_V.jpg'</span>, <span class="identifier">category_id</span><span class="symbol">:</span> <span class="identifier">art</span>.<span class="identifier"><span class="keymethods">id</span></span>)
</code></pre>
<p></p>
<p>As we can see, each of our Product columns has an entry. We also have <span style="background-color:rgb(204,204,204)">
<code>category_id: art.id</code></span>, referencing that this product belongs to the art category.</p>
** db/seeds.rb:<pre code_snippet_id="670722" snippet_file_name="blog_20150519_5_9983316"  code_snippet_id="670722" snippet_file_name="blog_20150519_5_9983316" name="code" class="ruby"># This file should contain all the record creation needed to seed the database with its default values.
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
   craft_supplies = Category.create(name: &#39;Craft Supplies&#39;, thumburl: &#39;http://bit.ly/1w1uPow&#39;)

   #Art
   Product.create(product_name: &#39;Russian Acrylic&#39;, description: &#39;Acrylic on Canvas&#39;, price: 59.00, thumburl: &#39;http://bit.ly/1nDkJZw&#39;, category_id: art.id)

   Product.create(product_name: &#39;Spanish Canvas Painting&#39;, description: &quot;La Fuente de Monteforte Painting Acrylic&quot;, price: 79.00, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/e/eb/144-la_fuente_de_Monforte_V.jpg&#39;,
      category_id: art.id)
   Product.create(product_name: &#39;French Acrylics &amp; Pastel Canvas&#39;, description: &quot;Jeanne d&#39;Arc Arrivant a l&#39;ile Bouchard&quot;, price: 122.00, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/3/36/2004_Yuri-Yudaev_Before-the-City-Gate_Acrylic-on-canvas_40x40cm.jpg&#39;, category_id: art.id)

   # Home &amp; Living

  Product.create(product_name: &#39;Art Deco Glass&#39;, description: &quot;Before-the-City-Gate Acrylic-on-canvas&quot;, price: 1599.00, thumburl: &#39;http://ihomedecorsideas.com/wp-content/uploads/2014/04/diy_network_homemade_coat_rack_.jpg&#39;, category_id: home_and_living.id)
   Product.create(product_name: &#39;Rustic Homemade Coatrack&#39;, description: &quot;Coatrack made of Maple Tree Branches&quot;, price: 288.00, thumburl: &#39;https://c2.staticflickr.com/6/5308/5821079295_4580e3c8d3_z.jpg&#39;, category_id: home_and_living.id)
   Product.create(product_name: &#39;Forest Wood Coffee Table&#39;, description: &quot;Chista Natural Wood Rustic Collection&quot;, price: 299.00, thumburl: &#39;https://c1.staticflickr.com/3/2777/4033647409_3c04157d86.jpg&#39;, category_id: home_and_living.id)

   # Jewelry

   Product.create(product_name: &#39;Vintage Rhinestone Earrings&#39;, description: &quot;Lightweight Rhinestone Earrings in Sterling Silver Setting&quot;, price: 9.00, thumburl: &#39;http://fc03.deviantart.net/fs70/f/2011/340/0/5/dangle_ear_rings_stock_png_by_doloresdevelde-d4idyev.png&#39;, category_id: jewelry.id)
   Product.create(product_name: &#39;Moon Turquoise Ring&#39;, description: &quot;Mediyat Silver&quot; , price: 39.99, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/f/ff/Midyat_Silver_Jewelry_1310103_Nevit.jpg&#39;, category_id: jewelry.id)
   Product.create(product_name: &#39;Greek Gold Necklace&#39;, description: &quot;Greek Gold Plated Necklace&quot;, price: 4570.00, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/0/02/Ancient_greek_jewelry_Staatliche_Antikensammlungen_Room_10_06.jpg&#39;, category_id: jewelry.id)


   # Women

   Product.create(product_name: &#39;Chloe Frill Yellow Dress&#39; , description: &quot;Vintage yellow dress with floral design. Lightweight with frill on skirt.&quot;, price: 59.99, thumburl: &#39;https://c1.staticflickr.com/9/8255/8660920433_57a184d9d1_z.jpg&#39;, category_id: women.id, )
   Product.create(product_name: &#39;Autumn Knitted Sweater with Silver Buttons&#39;, description: &quot;Knitted Crop Sweater with Silver Buttons&quot;, price: 45.99, thumburl: &#39;https://c2.staticflickr.com/4/3049/2353463988_c9d8cde436_z.jpg?zz=1&#39;, category_id: women.id)
   Product.create(product_name: &#39;Rucksack&#39;, description: &quot;Rucksack Schweizer Armee 1960.&quot;, price: 39.99, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/a/a1/Image-2D_and_3D_modulor_Origami.jpg&#39;, category_id: women.id)

   # Men

   Product.create(product_name: &#39;Grenson Shoes&#39;, description: &quot;Fullbrogue Grenson Shoes.&quot;, price: 105.00, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/d/d5/Fullbrogue_(Grenson).jpg&#39;, category_id: men.id)
   Product.create(product_name: &#39;Color Fringed Scarf&#39;, description: &quot;Men’s Fringed Scarf in Pumpkin Toffee Grey.&quot;, price: 19.99, thumburl: &#39;https://c2.staticflickr.com/4/3437/3832752067_c6c3631d44_z.jpg?zz=1&#39;, category_id: men.id)
   Product.create(product_name: &#39;Pork Pie Hat&#39;, description: &quot;Classic Pork Pie Hat from the 1940s.&quot;, price: 110.00, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/0/05/Brown_Porkpie_Hat.JPG&#39;, category_id: men.id)


   # Kids

   Product.create(product_name: &#39;Peruvian Hats&#39;, description: &quot;Handmade Peruvian Winter Hats for Children.&quot;, price: 15.00, thumburl: &#39;https://c2.staticflickr.com/8/7020/6498656815_3937483e21_z.jpg&#39;, category_id: kids.id)
   Product.create(product_name: &#39;Norev Toy Car&#39;, description: &quot;Classic Norev Model Toy Car for Children&quot;, price: 17.00, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/6/61/Norev_4cv.jpg&#39;, category_id: kids.id)
   Product.create(product_name: &#39;Stickle Bricks&#39;, description: &#39;Toy Stickle Brick Building Blocks Set&#39;, price: 21.99, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/f/f1/Stickle_bricks.jpg&#39;, category_id: kids.id)


   # Vintage

   Product.create(product_name: &#39;Anders Brown Leather Bag&#39;, description: &quot;Vintage Leather with White Trim.&quot;, price: 79.99, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/3/3d/Gesellenst%C3%BCck_Lederhandwerk.jpg&#39;, category_id: vintage.id)
   Product.create(product_name: &#39;Cambridge Red Footsies&#39;, description: &quot;Red Vintage Leather Shoes Children with Brown Laces and White Trim&quot;, price: 45.99, thumburl: &#39;https://c1.staticflickr.com/3/2587/3797274851_8199f17d01.jpg&#39;, category_id: vintage.id)
   Product.create(product_name: &#39;Voightlander Camera&#39;, description: &quot;Voightlander Vintage Camera with Metal Case&quot;, price: 179.00, thumburl: &#39;http://upload.wikimedia.org/wikipedia/commons/6/67/A_vintage_Voigtl%C3%A4nder_Vito_B_camera.jpg&#39;, category_id: vintage.id)

   # Weddings

   Product.create(product_name: &#39;Green Wedding Decor&#39;, description: &quot;Forest Dream Wedding Decoration Ideas&quot;, price: 27.00, thumburl: &#39;http://hostingessence.com/wp-content/uploads/2012/04/green-wedding.jpg&#39;, category_id: weddings.id)
   Product.create(product_name: &#39;Embossed Soap Wedding Favors&#39;, description: &quot;Lavendar Handmade Soap decorated with elegant nature design. &quot;, price: 4.50, thumburl: &#39;https://c1.staticflickr.com/1/203/518233215_cb4d2af38f_z.jpg?zz=1&#39;, category_id: weddings.id)
   Product.create(product_name: &#39;Handmade Centerpieces&#39;, description: &quot;Handmade Wedding Tea Cup Centerpieces for Your Guests&quot;, price: 35.00, thumburl: &#39;http://indiefixx.com/wp-content/uploads/2011/06/GR_teacupcenterpieces.jpg&#39;, category_id: weddings.id)

   # Craft Supplies

   Product.create(product_name: &#39;Korean Indasong Craft Paper&#39;, description: &quot;Origami Paper Kit&quot;, price: 17.00, thumburl: &#39;http://commons.wikimedia.org/wiki/File:Vesta_sewing_machine_IMGP0718.jpg&#39;, category_id: craft_supplies.id)
   Product.create(product_name: &#39;Handmade Gift Wrap&#39;, description: &quot;Esoterica Handmade Supplies&quot;, price: 14.00, thumburl: &#39;http://commons.wikimedia.org/wiki/File:Vesta_sewing_machine_IMGP0718.jpg&#39;, category_id: craft_supplies.id)
   Product.create(product_name: &#39;Lollipot Flavor Kit&#39;, description: &quot;Flavor Kit for Celebration Lollipops and Gifts&quot;, price: 21.00, thumburl: &#39;http://commons.wikimedia.org/wiki/File:Vesta_sewing_machine_IMGP0718.jpg&#39;, category_id: craft_supplies.id)
</pre><br>
In terminal type: bundle exec rake db:seed <br>
<pre class="jqconsole jqconsole-blurred" style="position:absolute; top:290px; bottom:0px; right:0px; left:-4px; margin:0px; overflow:auto; width:432px; height:101px"><span class="jqconsole-old-prompt"><span>$ bundle exec rake db:seed
</span></span><span class="jqconsole-prompt "><span></span><span><span>$ </span></span></span></pre>
<pre code_snippet_id="670722" snippet_file_name="blog_20150519_6_412573"  code_snippet_id="670722" snippet_file_name="blog_20150519_6_412573" name="code" class="plain">$ bundle exec rake db:seed
$ </pre>Now that we have a Model, we can create our Products Controller and Views. The Controller methods ensure that we serve dynamic content to the Products View. We can change the information users see.<br>
<h3>5. Create our Controller<br>
</h3>
<p>We saw this diagram when we created our Categories Controller. The good news is that our Products Controller will use the same pattern. We will use Create, Read, Update, and Delete to change information, commonly called CRUD.</p>
<p>Remember that five of these methods, index, show, new, edit, delete have Views as indicated here.</p>
<p>Take a look at the image below. It lists the Create, Read, Update, and Delete methods and what they do.<br>
&nbsp;&nbsp;&nbsp; <img src="http://img.blog.csdn.net/20150519164701520?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" height="554" width="643"><br>
</p>
<p>我们有Model了，要建立Products 的Controller，跟Category一样。<br>
我们要Products页面是动态的，包含8个methods, This way, we can 改变我们products的信息。<br>
我们就从5个有对应view的methods开始。在Terminal里：<br>
</p>
<pre><code> <span class="identifier">rails</span> <span class="identifier">generate</span> <span class="identifier">controller</span> <span class="identifier">products</span> <span class="identifier"><span class="keymethods">index</span></span> <span class="identifier">show</span> <span class="identifier"><span class="keymethods">new</span></span> <span class="identifier">edit</span> <span class="identifier"><span class="keymethods">delete</span></span>
</code></pre>
<p></p>
<p>Here we list the five main methods that have Views.</p>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_7_9661221"  code_snippet_id="670722" snippet_file_name="blog_20150520_7_9661221" name="code" class="plain">$ rails generate controller products index show new edit delete
create  app/controllers/products_controller.rb
 route  get &#39;products/delete&#39;
 route  get &#39;products/edit&#39;
 route  get &#39;products/new&#39;
 route  get &#39;products/show&#39;
 route  get &#39;products/index&#39;
invoke  erb
create    app/views/products
create    app/views/products/index.html.erb
create    app/views/products/show.html.erb
create    app/views/products/new.html.erb
create    app/views/products/edit.html.erb
create    app/views/products/delete.html.erb
invoke  test_unit
create    test/controllers/products_controller.rb
invoke  helper
create    app/helpers/products_helper.rb
invoke    test_unit
create      test/helpers/products_helper_test.rb
invoke  assets
invoke    coffee
create      app/assets/javascripts/products.js.coffee
invoke    scss
create      app/assets/stylesheets/products.css.scss
$  </pre>**<span>app/controllers/products_controller.rb</span>文件里，接下来就要写methods了。<br>
<p></p>
<h3>6. Add the method keep our data secure<br>
</h3>
We'll need to give our Products a secure way of fetching information from our Model.
<p></p>
<p>We'll create a strong parameters method that will keep our data secure. This private method prevents someone from altering the information we store in our Product Model.<br>
**<span>app/controllers/products_controller.rb</span>文件里，加入：</p>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_8_4517561"  code_snippet_id="670722" snippet_file_name="blog_20150520_8_4517561" name="code" class="ruby">private
def product_params  #strong params method, based on the name of the Model: &#39;product&#39;
 params.require(:product).permit(:product_name, :description, :price, :thumburl)#requires the Model name, and permits the columns
end
</pre>As you can see a strong params method (i.e. product_params) will be based on the name of the Model. Here, params requires the Model name (product) and permits the columns stored in our Migration table (product_name, description, price and thumburl).
<p></p>
<p></p>
<h3>7. Add the main controller methods: index, show, new, edit and delete<br>
</h3>
Now it's time to create our main controller methods again: <strong>index, show, new, edit</strong>and<strong> delete</strong>. Lucky for us, Rails is built on a series of patterns. We'll be using many of the same patterns we used before!<br>
三个文件：<br>
1. Controller： app/controllers/products_controller.rb<br>
2. Routes: config/routes.rb<br>
3. Views:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; index:&nbsp;&nbsp; &nbsp; app/views/products/index.html.erb<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; show:&nbsp;&nbsp; &nbsp; app/views/products/show.html.erb<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; new:&nbsp;&nbsp; &nbsp; app/views/products/new.html.erb<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; edit:&nbsp;&nbsp; &nbsp; app/views/products/edit.html.erb<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; delete:&nbsp;&nbsp; &nbsp; app/views/products/delete.html.erb<br>
=========<br>
&gt;&gt; 先准备好Routes:&nbsp; config/routes.rb<br>
----<pre code_snippet_id="670722" snippet_file_name="blog_20150520_9_2405543"  code_snippet_id="670722" snippet_file_name="blog_20150520_9_2405543" name="code" class="ruby">Rails.application.routes.draw do
  get &#39;/&#39; =&gt; &#39;pages#home&#39;

  resources :categories
  get &#39;categories/:id/delete&#39; =&gt; &#39;categories#delete&#39;, :as =&gt; :categories_delete

#this takes care of our main controller routes for products
  resources :products
  get &#39;products/:id/delete&#39; =&gt; &#39;products#delete&#39;, :as =&gt; :products_delete
end</pre>----
<p></p>
<p>&gt;&gt; 再一个个写Controller和Views<br>
</p>
<h4>&nbsp;&nbsp;&nbsp; 7.1 For index:</h4>
<p></p>
<p>&nbsp;&nbsp;&nbsp; Controller:<br>
&nbsp;&nbsp;&nbsp; ----</p>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_10_7785269"  code_snippet_id="670722" snippet_file_name="blog_20150520_10_7785269" name="code" class="ruby">    define index
      @products = Product.all
    end</pre>&nbsp;&nbsp;&nbsp; ----<br>
<br>
&nbsp;&nbsp;&nbsp; View:<br>
&nbsp;&nbsp;&nbsp; ----<pre code_snippet_id="670722" snippet_file_name="blog_20150520_11_4037048"  code_snippet_id="670722" snippet_file_name="blog_20150520_11_4037048" name="code" class="html">&lt;h1&gt;Listing products&lt;/h1&gt;

&lt;table&gt;
  &lt;thead&gt;
    &lt;tr&gt;
      &lt;th&gt;Name&lt;/th&gt;
      &lt;th&gt;Description&lt;/th&gt;
      &lt;th&gt;Price&lt;/th&gt;
      &lt;th&gt;Thumburl&lt;/th&gt;
      &lt;th colspan=&quot;3&quot;&gt;&lt;/th&gt;
    &lt;/tr&gt;
  &lt;/thead&gt;

  &lt;tbody&gt;
    &lt;% @products.each do |product|%&gt;
    &lt;!--# iterate through @products %&gt;--&gt;
      &lt;tr&gt;
        &lt;td&gt;&lt;%= product.product_name%&gt;&lt;/td&gt;
        &lt;td&gt;&lt;%= product.description %&gt;&lt;/td&gt;
        &lt;td&gt;&lt;%= product.price %&gt;&lt;/td&gt;
        &lt;% if product.thumburl %&gt;
          &lt;%= image_tag product.thumburl %&gt;
        &lt;% end %&gt;
        &lt;td&gt;&lt;%= link_to &#39;Show&#39;, product %&gt;&lt;/td&gt;
        &lt;td&gt;&lt;%= link_to &#39;Edit&#39;, edit_product_path(product) %&gt;&lt;/td&gt;
        &lt;td&gt;&lt;%= link_to &#39;Destroy&#39;, product, method: :delete, data: { confirm: &#39;Are you sure?&#39; } %&gt;&lt;/td&gt;
      &lt;/tr&gt;
    &lt;% end %&gt;
  &lt;/tbody&gt;
&lt;/table&gt;

&lt;br&gt;

&lt;%= link_to &#39;New Product&#39;, new_product_path %&gt;</pre>
<p></p>
<p>We're getting closer! The index page you made has all the right information, but this time around we added some style to the page. It now includes the supporting HTML for your Views.</p>
<p>Let's try adding the index view for categories again, now with style. Here again we would iterate through all Products and print out the name and the thumburl.<br>
**文件</p>
<div class="editor-filepath"><span class="fcn-icon fcn-icon-folder"></span><span></span><span>app/views/products/index.html.erb ：<br>
</span></div>
<p></p>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_12_6203"  code_snippet_id="670722" snippet_file_name="blog_20150520_12_6203" name="code" class="html">&lt;%= render &#39;shared/topnav&#39; %&gt;
&lt;br&gt;
&lt;br&gt;
&lt;br&gt;

&lt;h1&gt;Listing products&lt;/h1&gt;

&lt;% @products.in_groups_of(3).each do |products| %&gt;
  &lt;% products.select! {|x| !x.nil?} %&gt;
  &lt;div class=&#39;row&#39;&gt;
    &lt;% products.each do |product| %&gt;
      &lt;div class=&#39;col-md-4&#39;&gt;
        &lt;div class=&quot;thumbnail&quot;&gt;
            &lt;img src= &lt;%= product.thumburl %&gt; &gt;
            &lt;div class=&quot;caption&quot;&gt;
              &lt;span class=&quot;listing-title&quot;&gt;&lt;%= product.product_name %&gt;&lt;/span&gt;
              &lt;span class=&quot;listing-desc&quot;&gt;&lt;%= product.description %&gt;&lt;/span&gt;
              &lt;span class=&quot;listing-price&quot;&gt;&lt;%= number_to_currency(product.price) %&gt;&lt;/span&gt;
              &lt;span&gt;&lt;%= link_to &quot;Edit&quot;, edit_product_path(product.id) %&gt;&lt;/span&gt;
              &lt;span&gt;&lt;%= link_to &quot;Delete&quot;, products_delete_path(:id =&gt; product.id) %&gt;&lt;/span&gt;
            &lt;/div&gt;
          &lt;/div&gt;
      &lt;/div&gt;
    &lt;% end %&gt;
  &lt;/div&gt;
&lt;% end %&gt;
&lt;br&gt;

&lt;%= link_to &#39;New Product&#39;, new_product_path %&gt;



&lt;%= render &#39;shared/footer&#39; %&gt;
</pre><br>
<br>
<h4>&nbsp;&nbsp;&nbsp; 7.2For show method<br>
</h4>
<p>The show method finds an object by its id, and stores it in @product. Let's create the show method in our controller. Using our Pet example, it looks like this:</p>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_13_7927204"  code_snippet_id="670722" snippet_file_name="blog_20150520_13_7927204" name="code" class="ruby">  def show #request: localhost:8000/products/1
      @product = Product.find(params[:id])
  end</pre>We still need a view! We just want to show one product on a page. Let's go ahead and make the view for show.<br>
To do this, we'll be printing out the attributes that we stored in our instance variable @product. We add . and the attributes name after each attribute we print out.
<p></p>
<p>***<span>app/views/products/show.html.erb</span>:</p>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_14_3962398"  code_snippet_id="670722" snippet_file_name="blog_20150520_14_3962398" name="code" class="html">&lt;p&gt;
  &lt;strong&gt;Product name:&lt;/strong&gt;
  &lt;%= @product.product_name%&gt;
&lt;/p&gt;

&lt;p&gt;
  &lt;strong&gt;Description:&lt;/strong&gt;
  &lt;%= @product.description %&gt;
&lt;/p&gt;

&lt;p&gt;
  &lt;strong&gt;Price:&lt;/strong&gt;
  &lt;%= @product.price %&gt;
&lt;/p&gt;

&lt;p&gt;
  &lt;strong&gt;Thumburl:&lt;/strong&gt;
  &lt;%= @product.thumburl%&gt;
&lt;/p&gt;

&lt;%= link_to &#39;Edit&#39;, edit_product_path(@product) %&gt; |
&lt;%= link_to &#39;Back&#39;, products_path %&gt;
</pre><br>
<p></p>
<h4>&nbsp;7.3 Prepare&nbsp; _form.html.erb file</h4>
<p></p>
<p>在准备接下来的<strong>new/edit method</strong>之前，我们还需要准备好 _form.html.erb 文件：<span>app/views/products/_form.html.erb</span><br>
</p>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_15_8851756"  code_snippet_id="670722" snippet_file_name="blog_20150520_15_8851756" name="code" class="html">&lt;%= form_for(@product) do |f| %&gt;

  &lt;div class=&quot;product_name&quot;&gt;
		&lt;%= f.label :productname %&gt;&lt;br&gt;  &lt;!--为什么这里不能有空格呢？--&gt;
		&lt;%= f.text_field :product_name %&gt;
  &lt;/div&gt;
  &lt;div class=&quot;description&quot;&gt;
		&lt;%= f.label :description %&gt;&lt;br&gt;
		&lt;%= f.text_field :description %&gt;

  &lt;/div&gt;
  &lt;div class=&quot;price&quot;&gt;
		&lt;%= f.label :price %&gt;&lt;br&gt;
		&lt;%= f.text_field :price %&gt;

  &lt;/div&gt;
  &lt;div class=&quot;thumburl&quot;&gt;
		&lt;%= f.label :thumburl %&gt;&lt;br&gt;
		&lt;%= f.url_field :thumburl %&gt;

  &lt;/div&gt;
  &lt;div class=&quot;actions&quot;&gt;
		&lt;%= f.submit %&gt;

  &lt;/div&gt;
&lt;% end %&gt;
</pre><br>
<p></p>
<h4>7.4 Create the new method</h4>
<p></p>
<p>***<span> </span><span>app/controllers/products_controller.rb</span> :</p>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_16_3708096"  code_snippet_id="670722" snippet_file_name="blog_20150520_16_3708096" name="code" class="ruby">  def new  #localhost:8000/products/new
		@product = Product.new
  end
  def create
  	@product = Product.new(product_params)
  	if @product.save
  		redirect_to(:action =&gt; &#39;index&#39;)
  	else
  		render(&#39;new&#39;)
  	end
  end</pre><br>
<p>View: ***file: &nbsp; &nbsp;app/views/products/new.html.erb</p>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_17_3265300"  code_snippet_id="670722" snippet_file_name="blog_20150520_17_3265300" name="code" class="html">&lt;%= render &#39;shared/topnav&#39; %&gt;

&lt;h1&gt;New Product&lt;/h1&gt;

&lt;!-- Render form here --&gt;
&lt;%= render &#39;form&#39; %&gt;

&lt;%= link_to &#39;Back&#39;, products_path %&gt;

&lt;%= render &#39;shared/footer&#39; %&gt;
</pre><br>
<img src="http://img.blog.csdn.net/20150520230134641?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" height="421" width="392">
<p></p>
<p></p>
<h4>7.5 Create the edit method&nbsp;</h4>
<div>Controller: file ***&nbsp;&nbsp;app/controllers/products_controller.rb&nbsp;
<div class="editor-footer" style="margin:0px; padding:0px; border:0px; font-family:Oxygen,sans-serif; font-size:16px; vertical-align:baseline; word-break:break-word; width:413.34375px; color:rgb(32,64,86); background-color:rgb(42,48,52)">
<div class="fcn-tabs-container" style="margin:0px; padding:0px; border:0px; font-family:inherit; font-style:inherit; font-variant:inherit; font-weight:inherit; line-height:inherit; vertical-align:baseline; word-break:break-word">
</div>
</div>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_18_8187678"  code_snippet_id="670722" snippet_file_name="blog_20150520_18_8187678" name="code" class="ruby">  def edit  #localhost:8000/products/1/edit
		@product = Product.find(params[:id])
  end
  def update
  	@product = Product.find(params[:id])
  	if @product.update_attributes(product_params)
  		redirect_to(:action =&gt; &#39;show&#39;, :id =&gt; @product.id)
  	else
  		render(&#39;index&#39;)
  	end
  end</pre>
<p>&nbsp;View:<span style="font-family:Oxygen,sans-serif; font-size:14px; margin:0px; padding:0px; border:0px; vertical-align:baseline; word-break:break-word">&nbsp;</span><span style="font-family:Oxygen,sans-serif; font-size:14px; margin:0px; padding:0px; border:0px; vertical-align:baseline; word-break:break-word">app/views/products/edit.html.erb</span></p>
</div>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_19_1125190"  code_snippet_id="670722" snippet_file_name="blog_20150520_19_1125190" name="code" class="html">&lt;%= render &#39;shared/topnav&#39; %&gt;

&lt;h1&gt;Edit Product&lt;/h1&gt;

&lt;!-- Render form here --&gt;
&lt;%= render &#39;form&#39; %&gt;

&lt;%= link_to &#39;Back&#39;, products_path %&gt;

&lt;%= render &#39;shared/footer&#39; %&gt;</pre><br>
<img src="http://img.blog.csdn.net/20150520232252841?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" height="450" width="300"><br>
<p></p>
<p></p>
<h4>7.6 Create the delete method&nbsp;</h4>
<div>Controller: <br>
<pre code_snippet_id="670722" snippet_file_name="blog_20150520_20_9079211"  code_snippet_id="670722" snippet_file_name="blog_20150520_20_9079211" name="code" class="ruby">  def delete
  	@product = Product.find(params[:id])
  end
  
  def destroy
  	Product.find(params[:id]).destroy
  	redirect_to(:action =&gt; &#39;index&#39;)
  end</pre><br>
</div>
View:<span> ** app/views/products/delete.html.erb</span><br>
<pre code_snippet_id="670722" snippet_file_name="blog_20150521_21_9389882"  code_snippet_id="670722" snippet_file_name="blog_20150521_21_9389882" name="code" class="html">&lt;p&gt;
  &lt;strong&gt;Product name:&lt;/strong&gt;
  &lt;%= @product.product_name %&gt;
&lt;/p&gt;

&lt;p&gt;
  &lt;strong&gt;Description:&lt;/strong&gt;
  &lt;%= @product.description %&gt;
&lt;/p&gt;

&lt;p&gt;
  &lt;strong&gt;Price:&lt;/strong&gt;
  &lt;%= @product.price %&gt;
&lt;/p&gt;

&lt;p&gt;
  &lt;strong&gt;Thumburl:&lt;/strong&gt;
  &lt;%= @product.thumburl %&gt;
&lt;/p&gt;

&lt;%= link_to &#39;Delete&#39;, products_delete_path(:id =&gt; @product.id) %&gt; |
&lt;%= link_to &#39;Edit&#39;, edit_product_path(@product) %&gt; |
&lt;%= link_to &#39;Back&#39;, products_path %&gt;
</pre><br>
<h3>8. Conbine two Models together</h3>
<p></p>
<p>Before we finish, one final piece remains. We have a Products page and a Categories page, but right now they live entirely separate from one another.</p>
<p>Instead, we want our users to be able to go to a Category, click on show, and access the Products in that Category. If a user clicks on show in the Art Category, we should see the Art products that belong here.</p>
<p>We can do this by nesting嵌套 Products in our Categories show method. Since we have an association, we can list out all the products that belong to that category.<br>
为了让我们点击art这个category的show时，可以列出在art下的所有product，也就是要让product 按 art 分类。此时我们要把Products嵌套在Category下面，我们已经有association了，所以可以list out all the products that belong to that category.<br>
***<span> </span><span>app/controllers/categories_controller.rb</span><br>
</p>
<p></p>
<pre><code class="lang-rb"><span class="function"><span class="keyword">def</span> <span class="title">show</span></span>
  <span class="variable">@category</span> = <span class="constant">Category</span>.<span class="identifier"><span class="keymethods">find</span></span>(<span class="identifier">params</span>[<span class="symbol">:<span class="identifier"><span class="keymethods">id</span></span></span>])
  <span class="variable">@products</span> = <span class="variable">@category</span>.<span class="identifier">products #new code we write, this works cuz we hava an association
              #<span>store the products of a given category in an instance variable @product</span></span>
<span class="identifier"><span class="keyword">end</span></span>
</code></pre>
Here <span style="background-color:rgb(204,204,204)"><code>@products = @category.products</code></span> is the new code we write. This works because we have an association. Our code knows that certain products belong to a Category. We mentioned that Category
 in our seed data. <span style="color:#FF0000">这里好玄乎啊！</span>我们的代码知道某个产品是属于哪个category的，在我们的seed data里，我们提到了那个product所属的Category. 牛！所以，我们就加了这行代码，从而：<span>In categories_controller.rb, in the Categories show method, add code to store the products of a given category
 in an instance variable @product.</span><br>
<p></p>
<p><strong>除了嵌套Controller，还要嵌套nesting Views：</strong><br>
</p>
<p></p>
<p>The last and final step is to change the View for Categories show to be our Products index. This is because Categories show should now have all the Products for that Category.</p>
<p>Good news. We already wrote the code for this. All we need to do is pass it into a file. We can actually go ahead and<span style="color:#339999">replace the code we wrote for Categories show, with the code for Products index</span>.我们已经有了products &amp; categories的view，所以我们要做的是把它们pass
 into到一个file里。直接把Category show的代码替换成code for the Products index.<br>
</p>
<p>[Instructions:]<br>
&gt;&gt;<span>Copy all the code from your Products index in index.html.erb and paste it into your Categories show View in show.html.erb. All your previous code in Categories show can be replaced. 把”</span><span></span><span>app/views/products/index.html.erb</span>“中的代码，全部替换到”app/views/categories/show.html.erb“中，”app/views/categories/show.html.erb“中的代码可以不要了。<br>
&gt;&gt;<span>In your browser, visit localhost:8000/categories. On the Index page, under Art, click on Show. This will now show the Art Products in that Category.</span><br>
<span></span></p>
<span style="color:#999999">但是好像有点问题。点了没有列出art下面的所有东东。去到了一个categories/1<br>
<img src="http://img.blog.csdn.net/20150521113118163?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" height="438" width="775"><br>
</span>
<p>==========================</p>
<p></p>
<div class="accordion-section-body">
<div class="accordion-section-body__padded narrative--markdown">
<p>Congratulations! You created the Products page of the Etsy app. We created the Model, Controller, and Views for Products.</p>
<p>We generated the Product Model using our terminal. We used associations to define the relationship between the Category and Product table. We added Migration columns and migrated and seeded our database.</p>
<p>We then generated a Products Controller with the index, show, new, edit, and delete methods. This gave us route and View for those methods.</p>
<p>We created Controller methods for all eight of our CRUD actions, and Views for index, show, new, edit, and delete. We also nested our Products in our Categories show View.</p>
<p>Nice job. Now we'll finish our Etsy app by adding Authentication.</p>
</div>
</div>
<br>
<p></p>
<p><br>
</p>
</div>
</div>
   

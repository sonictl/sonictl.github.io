---
layout: post
title: 'Spring MVC 学习笔记12 —— SpringMVC+Hibernate开发（1）依赖包搭建'
date: 2014-11-26 07:37:00 +0800
category: from_cnblogs
slug: p20141126073700
---


<h3>Spring MVC 学习笔记12 —— SpringMVC&#43;Hibernate开发（1）依赖包搭建</h3>
<p><br>
</p>
<p>用Hibernate帮助建立SpringMVC与数据库之间的联系，通过配置DAO层，Service层，Model层，建立Controller对数据库操作的通道。</p>
<p>这里没有使用maven来管理jar包（依赖库），因为没太多，实际上还是很繁琐的，要有耐心。</p>
<p>原本稍微复杂的工程项目还是应该使用maven来管理依赖库，参见：<a target="_blank" target="_blank" href="http://tieba.baidu.com/p/2364606122?pn=1">http://tieba.baidu.com/p/2364606122?pn=1</a></p>
<p><br>
</p>
<p>下面记录一下依赖包搭建过程：</p>
<p>DAO - SERVICE -&nbsp;</p>
<p><br>
</p>
<h4>1.在eclipse中新建项目：</h4>
<p><span style="white-space:pre"></span>&nbsp; &nbsp;&nbsp;&nbsp;new-project-Dynamic Web Project（name: spring_user）</p>
<h4>2. insert jar packages:</h4>
&nbsp; &nbsp; &nbsp; 参考如图，导入jar包到目标位置<span style="white-space:pre"> </span>：<br>
<img src="http://img.blog.csdn.net/20141126151447350?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
<p>&nbsp; &nbsp; &nbsp; Download .jar files from:&nbsp;<a target="_blank" target="_blank" href="http://jarfiles.pandaidea.com">http://jarfiles.pandaidea.com</a>&nbsp; or&nbsp;<a target="_blank" target="_blank" href="http://mvnrepository.com/">http://mvnrepository.com/</a></p>
<p>&nbsp; &nbsp; &nbsp; 1)spring-reamework-release-3.1.3\dist</p>
<p><img src="http://img.blog.csdn.net/20141126151242296?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""></p>
<p><br>
</p>
<p><span style="white-space:pre"></span>2)apache-log4j-1.2.16.jar<br>
<img src="http://img.blog.csdn.net/20141126151755752?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
</p>
<span style="white-space:pre"></span>3)commons-logging-1.1.1 &nbsp; 用于登录<br>
<img src="http://img.blog.csdn.net/20141126151910632?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
<p><br>
</p>
<p><span style="white-space:pre"></span>4)hibernate-distribution-3.6.8 Final\lib\required（hibernate用3、4都可以）<br>
<img src="http://img.blog.csdn.net/20141126152140190?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
</p>
<p><br>
</p>
<p><span style="white-space:pre"></span>5)jpa<br>
<img src="http://img.blog.csdn.net/20141126152230297?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
</p>
<pre>    hibernate-jpa-2.0-api-1.0.0.Final.jar</pre>
<p><span style="white-space:pre"></span>6)hibernate3<br>
<img src="http://img.blog.csdn.net/20141126152405332?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
</p>
<pre>    hibernate3.jar
    slf4j-log4j12-1.6.1.jar</pre>
<p><span style="white-space:pre"></span>7)hlfa/dbcp/aop/jstl<br>
<img src="http://img.blog.csdn.net/20141126160808429?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
</p>
<pre>    aopalliance.jar
    aspectjrt.jar
    aspectjweaver.jar</pre>
<p></p>
<p>&nbsp; 8）dbcp 数据引导性文件<br>
<img src="http://img.blog.csdn.net/20141126161213531?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
</p>
<p></p>
<pre>&nbsp; &nbsp; commons-collections-3.1.jar
&nbsp; &nbsp; commons-dbcp.jar
&nbsp; &nbsp; commons-pool.jar

</pre>
&nbsp; 9）jstl 的标签：<br>
&nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141126161540721?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
<pre>    jsf-api.jar
    jsf-impl.jar
    jstl-1.2.jar

</pre>
&nbsp; &nbsp;10）数据库记录： &nbsp;<br>
<pre>&nbsp; &nbsp; mysql-connector-java-5.1.17-bin.jar</pre>
<p></p>
<p>&nbsp; &nbsp;<img src="http://img.blog.csdn.net/20141204101300625?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""></p>
<p><br>
&nbsp; ...</p>
<p></p>
<h3>3. 建立model层的Pakage:</h3>
&nbsp; &nbsp; &nbsp; &nbsp; New Java Package: zttc.itat.model，如图：
<p></p>
<p><img src="http://img.blog.csdn.net/20141126152645654?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
</p>
<br>
<h4>4. model下，建立一个类</h4>
<p>&nbsp; &nbsp; &nbsp; &nbsp; zttc.itat.model &gt;&gt; New Java Class</p>
<p><span style="white-space:pre"></span>&nbsp; &nbsp; &nbsp; &nbsp; Name: User<br>
&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141126152925000?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141126153114140?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
</p>
<p>----------</p>
<p></p>
<pre code_snippet_id="533584" snippet_file_name="blog_20141126_1_667878"  code_snippet_id="533584" snippet_file_name="blog_20141126_1_667878" name="code" class="java">   @Entity
   @Table(name=&quot;t_user&quot;)    //创建一张表
   pulic class User{ 
     private int id;
     private String username;
     private String password;
     private String email;
     private String nickname;  
   }

   @GenerateValue
   @Id
   public int getId(){
      return Id;
   }
   ...//后面就是其他参数的Get Set 方法</pre><br>
----------
<p></p>
<pre code_snippet_id="533584" snippet_file_name="blog_20141126_1_1533318"  code_snippet_id="533584" snippet_file_name="blog_20141126_1_1533318" name="code" class="java">
</pre><br>
<span style="white-space:pre"></span><strong>&nbsp; &nbsp; &nbsp; &nbsp; 1)add getter &amp; setter增加Get Setter:<br>
</strong><img src="http://img.blog.csdn.net/20141126153507616?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
&nbsp; &nbsp; &nbsp; &nbsp;Select all &gt;&gt; finish.<br>
<span style="white-space:pre"></span>&nbsp; &nbsp; &nbsp; &nbsp;2)add @Entity....<br>
<span style="white-space:pre"></span>
<p>&nbsp; &nbsp; &nbsp; &nbsp;此时我们再把分页的class加上： new package at zttc.itat.model &gt;&gt; new &gt;&gt; class &gt;&gt; Pager.java<br>
<img src="http://img.blog.csdn.net/20141203170420799?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="3" alt=""><br>
</p>
<h4>5. Spring 和Hibernate的整合</h4>
<p></p>
<p style="color:rgb(51,51,51); font-family:Arial; font-size:14px; line-height:26px">
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;打开数据库，新建一个database:spring_user&nbsp;<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;database charset:utf-8<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;database collection: utf-8<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<img src="http://img.blog.csdn.net/20141126162525187?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; <strong>bean.xml</strong>文件的coding：<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 从别处拷过来一个src\bean.xml:<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141126162447283?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="2" hspace="1" alt=""><br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;beans.xml -- 改为zttc.itat...<br>
</p>
<pre>&nbsp; &nbsp; &nbsp; &nbsp; 再改一下jdbc.properties：指定mysql数据库名： jdbc.url = jdbc:mysql://localhost:3306/spring_user</pre>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;参考（SpringMVC &#43; Spring &#43; SpringJDBC整合）：<a target="_blank" target="_blank" href="http://www.open-open.com/lib/view/open1349272132291.html">http://www.open-open.com/lib/view/open1349272132291.html</a></p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;参考：&nbsp;<a target="_blank" target="_blank" href="http://www.blogjava.net/yiqi801218/archive/2008/03/16/186670.html">http://www.blogjava.net/yiqi801218/archive/2008/03/16/186670.html</a><br>
</p>
<p><span style="white-space:pre"><img src="http://img.blog.csdn.net/20141126165119485?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""></span></p>
<br>
<br>
<h3>6. 写DAO层： zttc.itat.dao</h3>
&nbsp; &nbsp; &nbsp; &nbsp;<strong> 新建一个package</strong>, name: <strong>zttc.itat.dao</strong><br>
&nbsp; &nbsp; &nbsp; &nbsp;<img src="http://img.blog.csdn.net/20141126165409041?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""><br>
<p>&nbsp; &nbsp; &nbsp; &nbsp;</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp;<strong>再建一个Interface：IUserDao.java</strong><br>
&nbsp; &nbsp; &nbsp; &nbsp;<img src="http://img.blog.csdn.net/20141126165614812?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""></p>
&nbsp; &nbsp; &nbsp; &nbsp; 参考代码：<br>
&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<pre code_snippet_id="533584" snippet_file_name="blog_20141126_3_4619145"  code_snippet_id="533584" snippet_file_name="blog_20141126_3_4619145" name="code" class="java">//这里面有一些简单的方法：
    package zttc.itat.dao;
    public interface IUserDao {
        public void add(User user);
        public void update(User user);
        public void delete(int id);
        public User load(int id);
        public List&lt;User&gt; list();
	public Pager&lt;User&gt; find();  //分页的method声明
        public User loadByUsername(String username);

    }</pre><br>
&nbsp; &nbsp; &nbsp; &nbsp; 如图：<br>
&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141203171757055?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""><br>
&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<br>
<br>
<span style="white-space:pre"></span>
<p>&nbsp; &nbsp; &nbsp; &nbsp; <strong>再建立一个类：UserDao - UserDao.java, Add IUserDao里的方法进来：</strong></p>
<p><strong>&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141126171529913?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""></strong></p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; 完了我们要继承DaoSupport, 要注入SessionFactory, 还要自己写一个方法public void setsuperSessionFactory(). &nbsp;再用Repository来注入一下,名称：(&quot;userDao&quot;)。<br>
&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141126172059500?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="2" alt=""></p>
<p><br>
</p>
<h4>7. 写UserException（zttc.itat.modelpackage下）: New -&gt; Class -&gt; UserException (Superclass: java.lang.RuntimeException)</h4>
<div>&nbsp; &nbsp; &nbsp; &nbsp;UserException.java : Add default serial version ID</div>
<div>&nbsp; &nbsp; &nbsp; &nbsp;RightClick &gt;&gt; Source &gt;&gt; Generate Constructors from Superclass &gt;&gt; select all, 父类构造下来</div>
<div>&nbsp; &nbsp; &nbsp; &nbsp;UserException.java完成。<br>
&nbsp; &nbsp; &nbsp; &nbsp;<img src="http://img.blog.csdn.net/20141212180547159?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br>
</div>
<div>
<h4>8. 写SystemContext.java支持分页操作</h4>
<p>&nbsp; &nbsp; &nbsp; 传分页还得传当前页、每页显示多少条这几个数据传过来。所以要创建一个thread local</p>
<p>&nbsp; &nbsp; &nbsp; newClass: Source folder=spring_user/src, Package=zttc.itat.model &nbsp; name=SystemContext</p>
<p>&nbsp; &nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141129213407940?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="3" alt=""></p>
<div><br>
</div>
<br>
</div>
<h4>9. &nbsp;写Dao层各个方法，支持增删改查。文件：UserDao.java</h4>
<p>&nbsp; &nbsp; &nbsp; &nbsp;8.1 public void add(User user){ &nbsp; } &nbsp; 方法：<br>
</p>
<pre><span style="font-weight:normal">&nbsp; &nbsp; &nbsp; &nbsp; public void add(User user){
</span><span style="font-weight:normal">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; this.getHibernateTemplate().save(user);
</span><span style="font-weight:normal">        }</span></pre>
<img src="http://img.blog.csdn.net/20141129212551204?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="3" alt=""><br>
&nbsp; &nbsp; &nbsp; 在UserDao.java中再写Pager相关操作：
<p></p>
<h5>
<p>&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141128165920346?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="3" alt=""></p>
<p><span style="font-weight:normal"><span style="white-space:pre"></span>&nbsp; &nbsp; 如上图所示，完成add\update\delete\load\list等：</span></p>
<p></p>
<pre><span style="font-size:14px">&nbsp; &nbsp;return this.getSession().creatQuery(&quot;from User&quot;).list(); &nbsp; &nbsp; //下图补充一下上图。</span></pre>
<p></p>
</h5>
<p><img src="http://img.blog.csdn.net/20141203170912718?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""><br>
</p>
<p>再加一个方法：<br>
&nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141203172032015?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""></p>
<h3>接下来我们完成Service层 ===----------</h3>
<div><br>
</div>
<h4>10. 新建一个package: &nbsp;<strong>zttc.itat.service</strong> Package</h4>
<span style="white-space:pre"></span>&nbsp; &nbsp; &nbsp; 10.1 &nbsp; New Interface: IUserService.java/<br>
<img src="http://img.blog.csdn.net/20141204091728894?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""><br>
<br>
<h4>11. UserService.java</h4>
<p>&nbsp; &nbsp; &nbsp; 新建一个Class:</p>
<p>&nbsp; &nbsp; &nbsp;<img src="http://img.blog.csdn.net/20141204092028091?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""></p>
<p>&nbsp; &nbsp; &nbsp; Coding in UserService.java:</p>
<p><br>
</p>
<p><br>
</p>
<h4>12. 写SystemContextFilter</h4>
<p></p>
<p>&nbsp; &nbsp; &nbsp;新建一个Class:<strong>SystemContextFilter.java</strong></p>
<p><img src="http://img.blog.csdn.net/20141204093548937?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""></p>
&nbsp; &nbsp; &nbsp; &nbsp;这里有很多Coding的内容，视频时间：<img src="http://img.blog.csdn.net/20141204094122895?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" hspace="1" alt="">至：<br>
&nbsp; &nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141204094259218?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""><br>
<h4>15. 写web.xml</h4>
<p>&nbsp; &nbsp; &nbsp;先打开dispatcher:&nbsp;</p>
<p>&nbsp; &nbsp; &nbsp;再写mapping：</p>
<p>&nbsp; &nbsp; &nbsp;再拷字符编码部分：</p>
<p>&nbsp; &nbsp; &nbsp;<img src="http://img.blog.csdn.net/20141204092955948?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""></p>
<p>&nbsp; &nbsp; &nbsp;</p>
<p>&nbsp; &nbsp; &nbsp;还有把bean.xml加入监听：</p>
<p>&nbsp; &nbsp; &nbsp;OpenSessionInWiew</p>
<p>&nbsp; &nbsp; &nbsp;SystemContext</p>
<p>&nbsp; &nbsp; &nbsp;Filter（<span style="color:#ff0000">zttc.itat.web.SystemContextFilter</span>）</p>
<p>&nbsp; &nbsp; &nbsp;FilterMapping</p>
<p>&nbsp; &nbsp;&nbsp;</p>
<p><br>
</p>
<h4>15. 创建Spring的Bean文件： user-servlet.xml</h4>
<p>&nbsp; &nbsp;&nbsp;<img src="http://img.blog.csdn.net/20141204094610279?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""></p>
<p>&nbsp; &nbsp; &nbsp; beans, context, mvc这三个Desired XSD namespace declaration必须要选。</p>
<p>&nbsp; &nbsp; &nbsp; 在这里面要配这几个东西：参考：<a target="_blank" target="_blank" href="http://blog.csdn.net/kivenlee/article/details/6284732">http://blog.csdn.net/kivenlee/article/details/6284732</a></p>
<p></p>
<ul>
<li>mvc</li><li>context:component-scan</li><li>bean class=&quot;...InternalResourceViewResolver&quot;</li><li>其他异常那些东西就后面慢慢配了</li></ul>
<p></p>
<br>
<h3>新建Controller投入实际测试</h3>
<h4>16. 新建UserController.java</h4>
<img src="http://img.blog.csdn.net/20141204100920625?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""><br>
<p>&nbsp; &nbsp; &nbsp;<img src="http://img.blog.csdn.net/20141204101934118?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""></p>
<h4>17. 写WEB-INF/jsp/user/list.jsp</h4>
<img src="http://img.blog.csdn.net/20141204102108124?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""><br>
&nbsp; 写入： &lt;h1&gt;用户列表测试页面&lt;/h1&gt;<br>
<br>
<p>===========至此搞定，测试一下 ===========</p>
<p>Run &gt;&gt; Tomcat ，启动调试，http://localhost:8080/spring_user/user/users无问题。</p>
<p>看看数据库，也建起来了：</p>
<p><img src="http://img.blog.csdn.net/20141204102448349?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29uaWN0bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" border="1" hspace="5" alt=""><br>
</p>
<br>
<p>好累。。。</p>
   

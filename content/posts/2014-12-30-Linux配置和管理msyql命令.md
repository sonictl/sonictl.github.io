---
layout: post
title: 'Linux配置和管理msyql命令'
date: 2014-12-30 07:14:00 +0800
category: from_cnblogs
slug: p20141230071400
---


配置和管理msyql：<br>
　　1. 修改mysql最大连接数：cp support-files/my-mediuf，vim my.cnf，增加或修改max_connections=1024<br>
　　关于my.cnf：mysql按照下列顺序搜索my.cnf:/etc,mysql安装目录，安装目录下的data。/etc下的是全局设置。<br>
　　2. 启动mysql：/usr/local/mysql/bin/mysqld_safe --user=mysql &amp;<br>
　　查看mysql版本：mysqladmin -u root -p version<br>
　　注：网上安装或者二进制安装的可以直接使用如下命令启动和停止mysql: /etc/init.d/mysql start|stop|restart<br>
　　3. 停止mysql：mysqladmin -uroot -ppassw0rd shutdown 注意，u,p后没有空&#26684;<br>
　　4. 设置mysql自启动：把启动命令加入/etc/rc.local文件中<br>
　　5. 允许root远程登陆：<br>
　　1）本机登陆mysql：mysql -u root -p （-p一定要有）；改变数据库：use mysql;<br>
　　2）从所有主机：grant all privileges on *.* to root@&quot;%&quot; identified by &quot;passw0rd&quot; with grant option;<br>
　　3）从指定主机：grant all privileges on *.* to root@&quot;192.168.11.205&quot; identified by &quot;passw0rd&quot; with grant option; flush privileges;<br>
　　4) 进mysql库查看host为%的数据是否添加：use mysql; select * from user;<br>
　　6. 创建数据库，创建user：<br>
　　1) 建库：create database test1;<br>
　　2) 建用户，赋权：grant all privileges on test1.* to user_test@&quot;%&quot; identified by &quot;passw0rd&quot; with grant option;<br>
　　3）删除数据库：drop database test1;<br>
　　7. 删除权限：<br>
　　1) revoke all privileges on test1.* from test1@&quot;%&quot;;<br>
　　2) use mysql;<br>
　　3) delete from user where user=&quot;root&quot; and host=&quot;%&quot;;<br>
　　4) flush privileges;<br>
　　8. 显示所有的数据库：show databases; 显示库中所有的表：show tables;<br>
　　9. 远程登录mysql：mysql -h ip -u user -p<br>
　　10. 设置字符集（以utf8为例）：<br>
　　1） 查看当前的编码：show variables like 'character%';<br>
　　2)　修改my.cnf，在[client]下添加default-character-set=utf8<br>
　　3） 在[server]下添加default-character-set=utf8，init_connect='SET NAMES utf8;'<br>
　　4） 重启mysql。<br>
　　注：只有修改/etc下的my.cnf才能使client的设置起效，安装目录下的设置只能使server的设置有效。<br>
　　二进制安装的修改/etc/mysql/my.cnf即可<br>
　　11. 旧数据升级到utf8（旧数据以latin1为例）：<br>
　　1） 导出旧数据：mysqldump --default-character-set=latin1 -hlocalhost -uroot -B dbname --tables old_table &gt;old.sql<br>
　　2） 转换编码(Linux和UNIX)：iconv -t utf-8 -f gb2312 -c old.sql &gt; new.sql<br>
　　这里假定原表的数据为gb2312，也可以去掉-f，让iconv自动判断原来的字符集。<br>
　　3） 导入：修改new.sql，在插入或修改语句前加一句话：&quot;SET NAMES utf8;&quot;，并修改所有的gb2312为utf8，保存。<br>
　　mysql -hlocalhost -uroot -p dbname &lt; new.sql<br>
　　如果报max_allowed_packet的错误，是因为文件太大，mysql默认的这个参数是1M，修改my.cnf中的&#20540;即可（需要重启mysql)。<br>
　　12. 支持utf8的客户端：Mysql-Front,Navicat,PhpMyAdmin，Linux Shell（连接后执行SET NAMES utf8;后就可以读写utf8的数据了。10.4设置完毕后就不用再执行这句话了）<br>
　　13. 备份和恢复<br>
　　备份单个数据库：mysqldump -uroot -p -B dbname &gt; dbname.sql<br>
　　备份全部数据库：mysqldump -uroot -p --all-databases &gt; all.sql<br>
　　备份表： mysqldump -uroot -p -B dbname --table tablename &gt; tablename.sql<br>
　　恢复数据库：mysql -uroot -p &lt; name.sql<br>
　　恢复表：mysql -uroot -p dbname &lt; name.sql (必须指定数据库)<br>
　　14. 复制<br>
　　Mysql支持单向的异步复制，即一个服务器做主服务器，其他的一个或多个服务器做从服务器。复制是通过二进制日志实现的，主服务器写入，从服务器读取。可以实现多个主　　　　服务器，但是会碰到单个服务器不曾遇到的问题（不推荐）。<br>
　　1). 在主服务器上建立一个专门用来做复制的用户：grant replication slave on *.* to 'replicationuser'@'192.168.0.87' identified by 'iverson';<br>
　　2). 刷新主服务器上所有的表和块写入语句：flush tables with read lock; 然后读取主服务器上的二进制二进制文件名和分支：SHOW MASTER STATUS;将File和Position的&#20540;记录下来。记录后关闭主服务器：mysqladmin -uroot -ppassw0rd shutdown<br>
　　如果输出为空，说明服务器没有启用二进制日志，在my.cnf文件中[mysqld]下添加log-bin=mysql-bin，重启后即有。<br>
　　3). 为主服务器建立快照（snapshot）<br>
　　需要为主服务器上的需要复制的数据库建立快照，Windows可以使用zip&#26684;式，Linux和Unix最好使用tar命令。然后上传到从服务器mysql的数据目录，并解压。<br>
　　cd mysql-data-dir<br>
　　tar cvzf mysql-snapshot.tar ./mydb<br>
　　注意：快照中不应该包含任何日志文件或*.info文件，只应该包含要复制的数据库的数据文件（*.frm和*.opt）文件。<br>
　　可以用数据库备份(mysqldump)为从服务器做一次数据恢复，保证数据的一致性。<br>
　　4). 确认主服务器上my.cnf文件的[mysqld]section包含log-bin选项和server-id，并启动主服务器：<br>
　　[mysqld]<br>
　　log-bin=mysql-bin<br>
　　server-id=1<br>
　　5). 停止从服务器，加入server-id，然后启动从服务器：<br>
　　[mysqld]<br>
　　server-id=2<br>
　　注：这里的server-id是从服务器的id，必须与主服务器和其他从服务器不一样。<br>
　　可以在从服务器的配置文件中加入read-only选项，这样从服务器就只接受来自主服务器的SQL，确保数据不会被其他途经修改。<br>
　　6). 在从服务器上执行如下语句，用系统真实&#20540;代替选项：<br>
　　change master to MASTER_HOST='master_host', MASTER_USER='replication_user',MASTER_PASSWORD='replication_pwd',<br>
　　MASTER_LOG_FILE='recorded_log_file_name',MASTER_LOG_POS=log_position;<br>
　　7). 启动从线程：mysql&gt; START SLAVE; 停止从线程：stop slave;（注意：主服务器的防火墙应该允许3306端口连接）<br>
　　验证：此时主服务器和从服务器上的数据应该是一致的，在主服务器上插入修改删除数据都会更新到从服务器上，建表，删表等也是一样的。
   

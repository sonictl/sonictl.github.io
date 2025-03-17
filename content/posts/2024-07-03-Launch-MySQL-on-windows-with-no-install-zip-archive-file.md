---
layout: post
title:  "Launch MySQL on windows with no-install zip archive file"
date: 2024-07-03 21:26:24 +0800
tags: 
slug: p20240703212624
---


## 免安装 运行 mySQL 

1. 下载 v8.4 的MySQL文件包：https://dev.mysql.com/downloads/mysql/
   1. 选择版本 = 8.4.2 LTS
   2. 操作系统 = Microsoft Windows
   3. 下载 `Windows (x86, 64-bit), ZIP Archive`  （文件大小=247.5M）MD5= `9aad84967d8a94c390e76366ca85ec3c` 

2. 将文件 `mysql-8.4.2-winx64.zip` 解压，注意压缩包内所有文件放在`C:\mysql8`路径下。⚠️不要更改路径中的C盘

3. 确认在`C:\mysql8`路径下有文件夹`bin`、`docs`、`include`、`lib`、`share`，在`C:\mysql8`路径下新建配置文件`my.ini` （若已有，则仅编辑）

   用 [Notepad--](https://gitee.com/cxasm/notepad--/releases/tag/v2.19) 或 [vsCode](https://code.visualstudio.com/) 等文本编辑器，编辑`my.ini`文件内容如下：

   ```ini
   [mysqld]
   # 设置安装目录
   basedir = "C:\\mysql8"
   # 设置数据目录
   datadir = "C:\\mysql8\\data"
   sql_mode = NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
   #设置服务端的编码方式
   character-set-server=utf8mb4
   [client]
   #客户端编码方式，和服务端一致
   loose-default-character-set=utf8mb4
   # The port number to use when listening for TCP/IP connections. 
   # On Unix and Unix-like systems, the port number must be
   # 1024 or higher unless the server is started by the root system user.
   port = "3306"
   # Log errors and startup messages to this file.
   # log-error = "C:\\mysql8\\logs\\error_log.err"
   [WinMySQLadmin]
   Server = "C:\\mysql8\\bin\\mysqld.exe"
   
   ```
   
4. 将`C:\mysql8\bin`添加到 windows 系统环境变量。

   或者：

   “**以管理员身份**” 运行 PowerShell，在其中运行以下命令，以添加环境变量：

   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force
   $envVarName = 'Path'
   $envVarValueToAdd = 'C:\mysql8\bin'
   if ([Environment]::GetEnvironmentVariable($envVarName, 'Machine') -like "*$envVarValueToAdd*") {
       Write-Output "环境变量已存在。"
   }
   else {
       # Append the new value to the existing environment variable
       $currentValue = [Environment]::GetEnvironmentVariable($envVarName, 'Machine')
       if ($currentValue -eq $null) {
           $newValue = $envVarValueToAdd
       }
       else {
           $newValue = "$currentValue;$envVarValueToAdd"
       }
   
       # Set the updated environment variable
       [Environment]::SetEnvironmentVariable($envVarName, $newValue, 'Machine')
   
       # Check if the environment variable was successfully updated
       if ([Environment]::GetEnvironmentVariable($envVarName, 'Machine') -like "*$envVarValueToAdd*") {
           Write-Output "环境变量已成功添加。"
       }
       else {
           Write-Output "环境变量添加失败!"
       }
   }
   ```

   测试：管理员身份打开cmd窗口，运行`mysql --help` 以测试。若测试失败，或可重启计算机再试。

   

5. 安装。

   5.1: **初始化**：**管理员身份**运行cmd窗口，运行 以下命令，以初始化MySQL: 

   ```bash
    mysqld --defaults-file="C:\\mysql8\\my.ini" --initialize-insecure --user=mysql --console 
   ```

   执行完上面命令后，MySQL会在 `C:\mysql8` 路径下新建一个data文件夹，并且已在data文件夹建好默认数据库。用户可确认一下`C:\mysql8\data`文件夹内的内容。  此时，登录的用户名为root，密码为空。

   5.2: **安装服务**：等上一步执行完成，继续在cmd窗口中，运行 `mysqld install` 安装服务（“The servise may already exist” = “服务可能已经存在了”）。

   


6. 启动与停止MySQL服务：

​	 **启动服务**：继续在cmd窗口中(管理员身份)，运行 `net start mysql` 启动服务。此时即可参考下一节内容，操作mySql数据库。

​	 **停止服务**：继续在cmd窗口中(管理员身份)，运行 `net stop mysql` 停止服务。




## 使用mySQL服务

mySQL 服务启动后， 在cmd窗口运行`mysql -u root -p`, 密码为空（直接回车）。

看到 `mysql>`即可通过以下命令来使用数据库了：

```sql
SHOW DATABASES;
SELECT DATABASE();
USE mysql;
-- 建立表格：
CREATE TABLE test( 
  sno CHAR(10) PRIMARY KEY,
  sname VARCHAR(10)
                 );
-- 插入数据：
INSERT INTO test
VALUES 	('1000010000', '王一楠'),
				('1000010001', '王亚楠'),
				('1000010002', '王叔楠');
-- 查询：
SELECT * FROM test;
-- 退出：
QUIT;
```



#### ps: 修改密码：

首先需要切换使用 mysql 数据库：

```sql
mysql> USE mysql;
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';
```

这样的话 userName:root 对应的 pwd:root 方便好用。

刷新权限（**刷新后，密码才起效**）:

```sql
mysql> flush privileges;
mysql> exit;
```

这个刷新权限的命令一定要记住，经常会用到。

在cmd窗口再次运行 `mysql -u root -p`, 密码为 root 



#### 开启MySQL远程访问

 - 使用mysql的库，修改root用户的【host】为【%】(all)，再刷新权限即可完成远程访问权限的更改。

```sql
mysql> USE mysql;
mysql> SELECT user,authentication_string,Host from user;
mysql> UPDATE mysql.user SET host='%' WHERE user='root';   #修改root用户的【host】为【%】
mysql> SELECT user,authentication_string,Host from user;   #检查一下是否修改成功
mysql> flush privileges;   #刷新权限
```

 - cmd 远程连接确认：

    命令：`mysql -uroot -h <主机IP> -p`

    需要加上登录方式参数，`-h`来访问ip地址，命令行窗口显示如下：

    ```
    C:\Users\userName> mysql -u root -h 192.168.0.101 -p 
    
    C:\Users\userName> mysql -u root -h 192.168.0.101 -P 3306 -p
    # -h 192.168.0.101: 指定 MySQL 数据库服务器的主机名
    # -u root: 指定用户名为 root
    # -P: 指定端口号, 如不指定，默认 3306
    # -p: 提示用户输入密码
    ```

还可通过 DBeaver 或 Navicat 进行连接测试。



## 卸载MySQL

1. 停止服务`net stop mysql` 
2. 删除注册表：regedit
3. 路径1：`HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\EventLog\Application\MySQLD Service` , 删除`EventMessageFile 和 TypesSupported`
4. Opt:路径2：`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\MySQLD Service` , 删除`EventMessageFile 和 TypesSupported`
5. cmd执行命令 `mysqld -remove`, 看到【successfully removed】代表删除成功。
6. 移除环境变量中的`C:\mysql8\bin`，移除 `C:\mysql8` 下的data文件夹。



## Reference:

----

**[Reference Documentation : 2.3.5 Installing MySQL on Microsoft Windows Using a noinstall Zip Archive](https://dev.mysql.com/doc/refman/5.7/en/windows-install-archive.html)**

**Simplified Steps**

1. Download [MySQL Community Server 5.7.17 Windows (x86, 64-bit), ZIP Archive](https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.17-winx64.zip)

2. Extract the downloaded MySQL Server Archive to the desired location for MySQL server files (example : `D:\mysql\mysql-5.7.17-winx64`)

3. Create a directory for MySQL's database's data files (example : `D:\mysql\mydb`)

4. Create a directory for MySQL's database logging (example `D:\mysql\logs`)

5. Create MySQL options file (example location : `D:\mysql\config.ini`)

   ```sql
   # For advice on how to change settings please see
   # http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html
   
   [mysqld]
   
   # Remove leading # and set to the amount of RAM for the most important data
   # cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
   # innodb_buffer_pool_size = 128M
   
   # Remove leading # to turn on a very important data integrity option: logging
   # changes to the binary log between backups.
   # log_bin
   
   # These are commonly set, remove the # and set as required.
   # basedir = .....
   # datadir = .....
   # port = .....
   # server_id = .....
   
   
   # Remove leading # to set options mainly useful for reporting servers.
   # The server defaults are faster for transactions and fast SELECTs.
   # Adjust sizes as needed, experiment to find the optimal values.
   # join_buffer_size = 128M
   # sort_buffer_size = 2M
   # read_rnd_buffer_size = 2M 
   
   sql_mode = NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
   # set basedir to your installation path
   basedir = "D:\\mysql\\mysql-5.7.17-winx64"
   # set datadir to the location of your data directory
   datadir = "D:\\mysql\\mydb"
   # The port number to use when listening for TCP/IP connections. On Unix and Unix-like systems, the port number must be
   # 1024 or higher unless the server is started by the root system user.
   port = "55555"
   # Log errors and startup messages to this file.
   log-error = "D:\\mysql\\logs\\error_log.err"
   
   [mysqladmin]
   
   user = "root"
   port = "55555"
   ```

   - Selected port is 55555
   - `[mysqld]` groups options relating to mysqld.exe which will be used when mysql.exe reads this configuration file.
   - `[mysqladmin]` groups options relating to mysqladmin.exe which will be used when mysqladmin.exe reads this configuration file.

6. Initialize MySQL database files using Windows Batch File/Command Prompt (you might need [C++ redistribute](https://support.microsoft.com/en-us/topic/the-latest-supported-visual-c-downloads-2647da03-1eea-4433-9aff-95f26a218cc0) if you get an error)

   ```sql
   "D:\mysql\mysql-5.7.17-winx64\bin\mysqld.exe" --defaults-file="D:\\mysql\\config.ini" --initialize-insecure --console
   ```

- This will create a database files in the location specified in the configuration file.
  - It will have root user with no password
  - Error messages will be printed on current console window.

1. Create a batch file to start the MySQL database server

   ```sql
   "D:\mysql\mysql-5.7.17-winx64\bin\mysqld.exe" --defaults-file="D:\\mysql\\config.ini"
   ```

   - This will read `[mysqld]` part/group of the configuration file (`D:\mysql\config.ini`) and use options specified there to start the MySQL database server.

2. Create a batch file to shutdown the MySQL database server

   ```sql
   "D:\mysql\mysql-5.7.17-winx64\bin\mysqladmin.exe" --defaults-file="D:\\mysql\\config.ini" shutdown
   ```

   - This will read `[mysqladmin]` part/group of the configuration file (`D:\mysql\config.ini`) and use options specified there to specify and shutdown the MySQL database server.

3. You can now start your database and access it, and shut it down when it is not needed.

**DISCLAIMER** Those steps are supposed to help you get started with MySQL database and are in no way intended or secure for production.(root user doesn't even have a password set yet)

**Resources And More Details**

1. [Reference Documentation : 2.3.5 Installing MySQL on Microsoft Windows Using a noinstall Zip Archive](https://dev.mysql.com/doc/refman/5.7/en/windows-install-archive.html)
2. [Reference Documentation : 5.2.6 Using Option Files](https://dev.mysql.com/doc/refman/5.7/en/option-files.html)
3. [Reference Documentation : 5.2.3 Specifying Program Options](https://dev.mysql.com/doc/refman/5.7/en/program-options.html)
4. [Reference Documentation : 6.1.4 Server Command Options](https://dev.mysql.com/doc/refman/5.7/en/server-options.html)
5. [[Additional\] Reference Documentation : 5.6 Running Multiple MySQL Instances on One Machine](https://dev.mysql.com/doc/refman/5.5/en/multiple-servers.html)
6. [Steps to change root password](https://dev.mysql.com/doc/refman/5.7/en/default-privileges.html)

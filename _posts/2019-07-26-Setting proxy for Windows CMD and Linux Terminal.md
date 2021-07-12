---
layout: post
title: 'Setting proxy for Windows CMD and Linux Terminal'
date: 2019-07-26 02:17:00 +0800
category: from_cnblogs
slug: p20190726021700
---
> I found my blog is deleted by platform! WTF? I shall transfer all my blogs to a SECURE place! As soon as possible!! Jun 07, 2021

# setting proxy for Windows CMD and Linux Terminal

First ref(log in as owner): https://www.cnblogs.com/sonictl/diary/2021/06/07/14859534.html

### Linux Terminal:

set http_proxy=http://127.0.0.1:8118
set https_proxy=http://127.0.0.1:8118

set http_proxy=
set https_proxy=

more ref: [Setting proxy for apt from terminal](https://askubuntu.com/questions/158557/setting-proxy-for-apt-from-terminal)


### Windows CMD:

#### format:
set HTTP_PROXY=http://user:password@proxy.domain.com:port

#### examples:
set HTTP_PROXY=http://127.0.0.1:8118
set HTTPS_PROXY=http://127.0.0.1:8118


## Use Privoxy to transfer SOCKS to HTTP/HTTPS. 
or Use Proxifier to manage the proxy
## Brew安装 Install Privoxy on Mac OSX

输入命令:`brew install privoxy`就ok了。之后修改配置文件`/usr/local/etc/privoxy/config`,搜索`socks5`找到下面这一句:

```
#forward-socks5t   /               127.0.0.1:9050 .
```



其中9050是Tor浏览器提供的本地socks5端口，这句不用管，直接在下面加一句:

```
forward-socks5   /               127.0.0.1:1080 .
```



这里1080是本地socks5的端口，记住最后面有一个点号不能少.
另外还能看到默认的一句:

```
listen-address 127.0.0.1:8118
```



这表示privoxy只兼听本机的8118端口，如果你希望其他局域网内都可以用这个代理可以修改为:

```
listen-address 0.0.0.0:8118
```



## 开机自启

之前从来没有设置过开机自启，都是需要用到时才用脚本去启动，现在来看太麻烦了。完全可以让他开机启动，用到时去接到8118端口就可以了。
用brew安装完毕后，会看到下面一段话:

```
==> Installing privoxy
==> Downloading https://homebrew.bintray.com/bottles/privoxy-3.0.26.high_sierra.
######################################################################## 100.0%
==> Pouring privoxy-3.0.26.high_sierra.bottle.1.tar.gz
==> Caveats
To have launchd start privoxy now and restart at login:
  brew services start privoxy
Or, if you don't want/need a background service you can just run:
  privoxy /usr/local/etc/privoxy/config
```



如果想设置开机启动的话，只需要:`sudo brew services start privoxy`就ok了。如果单次启动的话用`/usr/local/sbin/privoxy /usr/local/etc/privoxy/config`。
**备注:这个启动方式和用[Privxoy](https://www.privoxy.org/)原始安装方式还是有区别的。**

## 验证是否启动

### 方式一

经过上述操作打开shadwosocks和privoxy后，就需要验证socks5代理转http是否成功。首先看privoxy是否启动起来了,输入`ps -ef|grep privoxy`可以看到:

```
501   400     1   0  4:58下午 ??         0:00.18 /usr/local/Cellar/privoxy/3.0.26/sbin/privoxy --no-daemon /usr/local/etc/privoxy/config
501   402     1   0  4:58下午 ??         0:00.01 /Users/yanzi/Library/Application Support/ShadowsocksX-NG/privoxy --no-daemon privoxy.config
501  1625  1600   0  6:46下午 ttys001    0:00.00 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn privoxy
```



可以看到已经启动成功了。

### 方式二

输入`netstat -an | grep 8118`可以看到:

```
tcp4       0      0  127.0.0.1.8118         *.*                    LISTEN
```



也能证明已经ok了。

### 方式三

最根本的方法莫过于通过8118端口访问baidu了:

```
wget -e http_proxy=127.0.0.1:8118 www.baidu.com
curl -x 127.0.0.1:8118 www.baidu.com
curl -x 127.0.0.1:8118 ip.sb  #这个返回一个外网ip
```



用wget和curl都是ok的。

## 终端也能用代理

在`~/.zshrc`里加上两句:

```
export http_proxy='http://127.0.0.1:8118'
export https_proxy='http://127.0.0.1:8118'
```



通过`echo $http_proxy`要确保其生效.
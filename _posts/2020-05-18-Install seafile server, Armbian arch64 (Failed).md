---
layout: post
title: 'Install seafile server, Armbian arch64 (Failed)'
date: 2020-05-18 13:58:00 +0800
category: from_cnblogs
slug: p20200518135800
---
# Install seafile server, Armbian arch64

**I write this to ask you to give it up. This is a note with failed result. Just use docker if you wanna quick&smooth deployment.**

#### Download binary package
Visit our [download page](http://www.seafile.com/en/download/#server), download the **latest** server package:

reference:

   https://seafile.gitbook.io/seafile-server-manual/deploying-seafile-under-linux/deploying-seafile-with-sqlite

   https://forum.seafile.com/t/compilation-arm64-done/6170

   

```bash
cd
mkdir -p Downloads/seafile
wget hhttps://cdn.iklive.eu/tcb13/2018/seafile-server_6.3.0_arm64.tar.gz

```

#### Deploy

you may need `sudo -i` first

```bash
mkdir haiwen
mv seafile-server_*.tar.gz haiwen
cd haiwen
# after moving seafile-server_* to this directory
tar -xzf seafile-server_*.tar.gz
mkdir installed
mv seafile-server_*.tar.gz installed
```



#### Setting up seafile server:

```bash
# As the default python binary on Debian server is python 3(check by `phthon3 -V`), we need to install python (python 2) first.
apt-get update
apt-get install python
apt-get install python2.7 libpython2.7 python-setuptools python-pil python3-pil python-ldap python-urllib3 ffmpeg python-pip sqlite3 python-requests
pip install pillow
pip install moviepy  # For movie thumbnail, ignore. This may cause error
```



**Seafile 6.3.x and above versions**

setup seafile by run `./setup-seafile.sh`


```txt
server name:        seafile-chBox
server ip/domain:   localhost
seafile data dir:   /data/seafile-data
port of seafile fileserver:   8082
port of seahub:               8000
```

You will see:

```txt
-----------------------------------------------------------------
Your seafile server configuration has been completed successfully.
-----------------------------------------------------------------

run seafile server:     ./seafile.sh { start | stop | restart }
run seahub  server:     ./seahub.sh  { start <port> | stop | restart <port> }

-----------------------------------------------------------------
If the server is behind a firewall, remember to open these tcp ports:
-----------------------------------------------------------------

port of seafile fileserver:   8082
port of seahub:               8000
```

#### start ther server

```bash
./seafile.sh start
./seahub.sh start <port>
```

you may meet error when start seafilehub, try `./seahub.sh start-fastcgi` to see the detail.

if you meet `No module named appconf ` error, try `pip install django-appconf==1.0.2 `to install them.

Eventhough your two .sh files starts without error, you can still has no response when you try to login the web page on hostname:port

#### check the port occupation

when you success on starting the two .sh files, check the port occupation:

```bash
# lsof -i -P -n | grep LISTEN
sshd       894    root    3u  IPv4  12730      0t0  TCP *:22 (LISTEN)
sshd       894    root    4u  IPv6  10982      0t0  TCP *:22 (LISTEN)
seaf-serv 6121    root   18u  IPv4  28113      0t0  TCP *:8082 (LISTEN)
python2.7 6505    root    3u  IPv4  37061      0t0  TCP 127.0.0.1:8000 (LISTEN)
python2.7 6506    root    3u  IPv4  37061      0t0  TCP 127.0.0.1:8000 (LISTEN)
python2.7 6507    root    3u  IPv4  37061      0t0  TCP 127.0.0.1:8000 (LISTEN)
python2.7 6508    root    3u  IPv4  37061      0t0  TCP 127.0.0.1:8000 (LISTEN)
python2.7 6509    root    3u  IPv4  37061      0t0  TCP 127.0.0.1:8000 (LISTEN)
python2.7 6510    root    3u  IPv4  37061      0t0  TCP 127.0.0.1:8000 (LISTEN)
```

#### kill the process

```bash
./seahub.sh stop
./seafile.sh stop
```

modify the value of SERVICE_URL in the file `../conf/ccnet.conf`, like this: (assume your ip or domain is 192.168.1.100). You can also modify SERVICE_URL via web UI in "System Admin->Settings". (**Warning**: if you set the value both via Web UI and ccnet.conf, the setting via Web UI will take precedence.)



You can assign the port of Seahub by setting the `conf/gunicorn.conf`.

```

```

## Conclusion: The process of configuration is really tough.
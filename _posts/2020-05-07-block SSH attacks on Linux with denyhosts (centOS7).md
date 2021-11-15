---
layout: post
title: 'block SSH attacks on Linux with denyhosts (centOS7)'
date: 2020-05-07 09:50:00 +0800
category: from_cnblogs
slug: p20200507095000
---
## How To Install and Configure DenyHosts in CentOS 7 for preventing the violent cracking on ssh
### 防止ssh被暴力破解：denyhost工具

DenyHosts is an open-source python-based tool developed by Phil Schwartz intended to prevent brute-force attacks and dictionary-based attacks on SSH servers. It is a log-based security tool.
It is simple and the ability to configure rules manually.

DenyHosts is widely used as an alternative to Fail2ban. If there have been too many invalid SSH login attempts it assume that it’s brute-force attacks or dictionary-based attacks and prevents the IP address from making any further attempts by adding it to /etc/hosts.deny on the server.

**([Learn To Install Fail2ban on CentOS 7](https://www.hostnextra.com/kb/learn-to-install-fail2ban-on-centos-7/))**

#### Install and configure denyhost in centos 7 from Github Repo

First, install git

> \# yum install git

Now, cloud the [denyhosts Github](https://github.com/denyhosts/denyhosts) repo

>\# cd /usr/share/
> \# git clone https://github.com/denyhosts/denyhosts

After cloning the files from the Github, install it.

> \# cd denyhosts
>
> \# python setup.py install

This will install the DenyHosts modules into python’s site-packages directory.

#### Let’s configure DenyHosts

We need to create a configuration file before it can function. The sample configuration file denyhosts.conf contains most of the possible settings and we need to copy it and edit the file.

> \# cp denyhosts.conf /etc
>
> \# vim /etc/denyhosts.conf

The sample configuration file contains informational comments that should help you quickly configure DenyHosts. 

**Note:** Edit by commenting out the  `#` at the lines with words `Redhat` for centOS7.

After you have edited your configuration file, save it.

Next, we need to copy the sample daemon-control.dist script as such to daemon-control and modify the recommended section as per your configuration file location.

> \# cp daemon-control-dist daemon-control

Edit the daemon-control file. You should only need to edit this section near the top:

![install denyhosts in centos 7](https://img2020.cnblogs.com/blog/780771/202005/780771-20200507175010570-126865914.png)


> \###############################################
> \#### Edit these to suit your configuration ####
> \###############################################
>
> DENYHOSTS_BIN = “/usr/local/bin/denyhosts.py”
> DENYHOSTS_LOCK = “/var/lock/subsys/denyhosts”
> DENYHOSTS_CFG = “/etc/denyhosts.conf”

Once you have edited the configuration and daemon control files make sure that the daemon control script it executable.

#### Now start DenyHosts manually

> \# ./daemon-control start

You should refer to the daemon log (typically /var/log/denyhosts) to ensure that DenyHosts is running successfully.

#### To start DenyHosts automatically

Create a symbolic link from /etc/init.d such as:

> \# cd /etc/init.d
> \# ln -s /usr/share/denyhosts/daemon-control denyhosts

Now, we create a [symbolic link](https://linuxize.com/post/how-to-remove-symbolic-links-in-linux/) and we can manage denyhosts service from systemctl command

To enable DenyHosts

> \# systemctl enable denyhosts

To start

> \# systemctl start denyhosts

To check the status

> \# systemctl status denyhosts

### Starting DenyHosts Automatically
#### Method 1 (preferred)
Create a symbolic link from /etc/init.d such as:

> \# cd /etc/init.d
> \# ln -s /usr/share/denyhosts/daemon-control denyhosts
If you have chkconfig installed you can then use it to ensure that DenyHosts runs at boot time:

> \# chkconfig --add denyhosts
If you do not have chkconfig (or similar) installed you can either manually create the symlinks in /etc/rc2.d, /etc/rc3.d, /etc/rc5.d but that is beyond the scope of this document.

#### Method 2
Add an entry into the /etc/rc.local file:

> /usr/share/denyhosts/daemon-control start
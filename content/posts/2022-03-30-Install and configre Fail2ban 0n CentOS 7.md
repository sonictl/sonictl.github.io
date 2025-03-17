---
layout: post
title:  "Install and Configure Fail2Ban On CentOS 7"
date: 2022-03-30 11:09:04 +0800
categories: jekyll
slug: p20220330110904
---
# Install Fail2Ban On CentOS 7



Posted on December 13, 2019 by [David Singer](https://www.liquidweb.com/kb/author/dsinger/)



## What Is Fail2Ban?

Fail2ban is an open-source software that actively scans the servers log files in real-time for any brute force login attempts, and if found, summarily blocks the attack using the servers firewall software (firewalld or iptables). Fail2Ban runs as a background process and continuously scans the log files for unusual login patterns and security breach attempts.

## Installation

In order to install Fail2Ban on CentOS 7, we first need to enable the EPEL (Extra Packages for Enterprise Linux) repository. The following commands will be run as the root user.

```
[root@host ~]# yum install epel-release
[root@host ~]# yum install fail2ban fail2ban-systemd
```

We can also [install Fail2ban](https://github.com/fail2ban/fail2ban) by [cloning](https://www.liquidweb.com/kb/create-clone-repo-github-ubuntu-18-04/) the software from GitHub.

### Configuration

Once fail2ban is installed, we need to configure and adjust the software with an updated jail.local configuration file. As a side note, the fail2ban software stores its configuration files within the /etc/fail2ban folder. The jail.local file supersedes the jail.conf file and is normally used to verify your custom updates are safe. 

First, we will make a copy of the “jail.conf” file and re-save it with the name “jail.local”:

```
[root@host ~]# cp -pf /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

Next, we will need to edit the jail.local file in Vim using the following command.

```
[root@host ~]# vim /etc/fail2ban/jail.local
[DEFAULT]

# "ignoreip" can be an IP address, a CIDR mask or a DNS host. Fail2ban will not
# ban a host which matches an address in this list. Several addresses can be
# defined using space separator.
ignoreip = 127.0.0.1

# Override /etc/fail2ban/jail.d/00-firewalld.conf:
banaction = iptables-multiport

# "bantime" is the number of seconds that a host is banned.
bantime  = 600

# A host is banned if it has generated "maxretry" during the last "findtime"
# seconds.
findtime  = 600

# "maxretry" is the number of failures before a host get banned.
maxretry = 5
```

Add your local IP address onto the ***ignoreip*** line.

## Add A Jail File To Protect SSH Access

Next, we need to create and edit a new file in Vim called sshd.local.

```
[root@host ~]# touch /etc/fail2ban/jail.d/sshd.local && chmod +x /etc/fail2ban/jail.d/sshd.local

[root@host ~]# vim /etc/fail2ban/jail.d/sshd.local
```

After this, we add the following lines of code.

```
[sshd]
enabled = true
port = ssh
#action = firewallcmd-ipset
logpath = %(sshd_log)s
maxretry = 5
bantime = 86400
```

Ensure that the parameter ‘enabled’ is set to ‘true’. In order to enable protection, set to True, To disable protection, set it to False. The filter parameter on line 167 in the jail.conf file

```
filter = %(__name__)s[mode=%(mode)s]
```

checks the sshd configuration file against this setting located in the path /etc/fail2ban/filter.d/sshd.conf.

The action of this parameter is used to define the IP address needing to be banned using the filter available in the /etc/fail2ban/action.d/firewallcmd-ipset.conf file.

Additionally, the SSH port parameter may be modified to a new value to match your SSH settings. If you are using port 22, there is no need for a change in this parameter. Other parameters include:

- Logpath: Logpath defines the path where the log file will be stored. This log file is scanned by Fail2Ban.
- Maxretry: Maxetry is used to define the max limit of failed login entries.
- Bantime: The bantime parameter is used to define the number of seconds a host will be banned.

## Fail2Ban Services

If you are not running the CentOS Firewall ([firewalld](https://www.liquidweb.com/kb/how-to-start-and-enable-firewalld-on-centos-7/)), enable it:

```
[root@host ~]# systemctl enable firewalld
[root@host ~]# systemctl start firewalld
```

Run the following commands to enable and start the Fail2Ban software on the server.

```
[root@host ~]# systemctl enable fail2ban
[root@host ~]# systemctl start fail2ban
```

## Fal2Ban Status

To check the status of the Fail2Ban jails, run the following command:

```
[root@host ~]# fail2ban-client status
```

The result should be similar to this:

```
[root@host ~]# fail2ban-client status
Status
|- Number of jail: 1
`- Jail list: sshd
```

## Unbanning An IP Address

In order to manually remove an IP address from the banned list, use the following command:

```
[root@host ~]# fail2ban-client set sshd unbanip IPADDRESS
```

This will remove the IP.


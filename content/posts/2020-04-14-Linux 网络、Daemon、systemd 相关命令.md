---
layout: post
title: 'Linux 网络、Daemon、systemd 相关命令'
date: 2020-04-14 04:14:00 +0800
category: from_cnblogs
slug: p20200414041400
---
# === CentOS 7+ 网络、Daemon 相关命令 ===
包含了centos system 中 firewalld, port occupation, systemd service management 相关常用命令。

### 1. firewalld
	`firewall-cmd --permanent --add-port=100/tcp ` 	# open specific port
	`firewall-cmd --permanent --add-port=200-300/tcp`
	`firewall-cmd --permanent --add-service=http`		# open port for service
	`firewall-cmd --reload`  							# take changes into effect
	`firewall-cmd --list-ports`   					# check the ports opened.
	`firewall-cmd --list-services`					# check the service
	` iptables-save`									# check all currently applied rules
	
### 2. port
	netstat -tlunp							# display all ports opened in host
	lsof -i									# list the network open ports 
	netstat -lptu							# the process that owns them
	netstat -tulpn							# You can add "     | grep "kw1\|kw2\|kw3'     " for keywords grep
	
	
### 3. daemon 守护与开机（开机启动可用）
	vim /etc/rc.d/rc.local					# edit script in this file for script running at boot
	vim ~.bash_profile 						# exe script at log on.	and ~.bash_logout for log out.
	shutdown -r now
	systemctl reboot						# Reboot to verify scripts launching during system boot
	
### 4. systemd（开机启动可用）
	vim  /etc/systemd/system/sample.service 
```
		[Unit]
		Description=Description for sample script goes here
		After=network.target

		[Service]
		Type=simple
		ExecStart=/var/tmp/test_script.sh
		TimeoutStartSec=0

		[Install]
		WantedBy=default.target
```
	systemctl daemon-reload
	systemctl enable sample.service
	systemctl start sample.service
	systemctl list-unit-files | grep enabled  	# list all enabled ones.
	systemctl list-unit-files | grep disabled 	# list all disabled ones.

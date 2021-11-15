---
layout: post
title: 'Deploy NextCloudPi (ncp) docker on aarch64 (arm64) board'
date: 2020-05-18 12:45:00 +0800
category: from_cnblogs
slug: p20200518124500
---
# Deploy NextCloudPi(ncp) docker on aarch64 board

## 在Arm64或aarch64架构的板卡上使用Docker布署NextCloudPi云盘服务

> About "arm64 vs aarch64", just Google it.


<p style="text-align: center;"><img src="https://img2020.cnblogs.com/blog/780771/202005/780771-20200518203347990-367555538.png" alt="" width="350" height="230" />&nbsp;<img src="https://img2020.cnblogs.com/blog/780771/202005/780771-20200518204317981-246737653.png" alt="" width="420" /></p>



<p style="text-align: center;"><a href="http://av98.byethost10.com/index.php?thread-457.htm&amp;i=1" target="_blank" rel="noopener">figure source</a></p>

### Reference:

 - Get started with NextCloudPi (ncp) installation via docker: https://docs.nextcloudpi.com/en/how-to-get-started-with-ncp-docker/
 - Docker installation, on aarch64, ref: https://www.cnblogs.com/sonictl/p/12909874.html

### 1. Install ncp with docker:

If you wanna firstly have a taste in LAN network env.:

```bash
docker run -d -p 4443:4443 -p 443:443 -p 80:80 -v ncdata:/data --name nextcloudpi ownyourbits/nextcloudpi 127.0.0.1
```

If you have a domain name:
```
docker run -d -p 4443:4443 -p 443:443 -p 80:80 -v ncdata:/data --name nextcloudpi ownyourbits/nextcloudpi $YOUR_Domain
```



### 2. Some useful commands for work with docker:
We recommend to firstly read all the content below, and pick commands for your need during configuration. The procedures of configuration is roughly as below:

##### Enter the container:
```bash
  docker exec -it <container name> /bin/bash
  docker exec -it 6ef91daa0481 /bin/bash
```



##### Set the timeZone same with host:

  **Note: failing synchronizing the timezone of docker-container and host may lead Authorization Fatal**
This is a big entraping‼️ 😭

```bash
  cp /usr/share/zoneinfo/PRC /etc/localtime
  docker exec -it 6ef91daa0481 date
  docker exec -it 6ef91daa0481 /bin/bash
```



##### Install vim for container

  You may need to configure `config/config.php`, but we recommend to use `ncp-config` as below.

```bash
  apt-get update
  apt-get install vim
  vim /var/www/nextcloud/config/config.php
  docker exec -it nextcloudpi ncp-config  #WARNING: note the '_' everywhere in 'ncp-config'!!
```
  ref: https://help.nextcloud.com/t/reset-password-ncp/74207/12

##### Enlarge the volume for '/var/lib/docker' directory
  WARNING: data may get lost. Do this if you have no data in your ncp account. Do it on your own risk.
  WARNING: data may get lost. Do this if you have no data in your ncp account. Do it on your own risk.

  ref: https://forums.docker.com/t/how-to-change-var-lib-docker-directory-with-overlay2/43620/9

  1. make the new folder on the other volume, e.g. you mount your Mess-storage HardDisk /sda1 on /data
  2. stop the docker daemon,
  3. move everything, e.g. `cp -avr /var/lib/docker/ /data/lib/`
  4. Edit the file /etc/docker/daemon.json and add or modfy the “data-root” entry. 
    If you configuration is empty, the file will look like this:
```
	{
	  "data-root": "/data/lib/docker"
	}
```
  5. Stop the docker daemon by `systemd stop docker`
  6. modify the configuration file for containers: /data/lib/docker/containers/<container full id>/config.vs.json. At the `source:`, make it into your new path.
  7. start the docker daemon.
  8. Check 
      - Check by: `# docker info | grep Root && docker ps -a && docker image ls`
      - start the docker container
    If you meet the data-lost error, just change `"data-root": "/data/lib/docker"` back.
> If you meet the error below:
> Error response from daemon: error evaluating symlinks from mount source "/var/lib/docker/volumes/esdata/_data": lstat /var/lib/docker: no such file or directory
>
>>ref links and modify the configuration of docker container.
>>ref link1: [meet error after moving docker root](https://forums.docker.com/t/error-starting-container-after-moving-docker-root-directory/64324)
>>ref link2: [change container configuration](https://bobcares.com/blog/docker-change-container-configuration/)
>>ref link3: [how to format json file in vim](https://vi.stackexchange.com/questions/16906/how-to-format-json-file-in-vim)




##### set the frpc for exposing the local service to public Internet
  - Check the port occupation:
  ```bash
    # lsof -i -P -n | grep LISTEN
	docker-pr  9168    root    4u  IPv6 120391      0t0  TCP *:4443 (LISTEN)
	docker-pr  9183    root    4u  IPv6 119704      0t0  TCP *:443 (LISTEN)
	docker-pr  9197    root    4u  IPv6 122113      0t0  TCP *:80 (LISTEN)
  ```
  - config `frpc`: `vim /etc/frp/frpc.ini`

    If you're not familiar frp(frps & frpc), learn and be practised about it.
  ```
  e.g.    
      remote: 8282
      local:  80
  ```

  - domain name recored: map `cloud.yourdomain.com` to the IP of frps's host.

  - in frps's host, add host `cloud.yourdomain.com` to `8282`:   `vim /etc/caddy/Caddyfile`

    > Caddy 自动https，确实很香啊~！Nginx 的反向代理配置啥的，这里就按下不表了哈~
  ```
  cloud.yourdomain.com {
        gzip
        log stdout
        proxy / localhost:8282 {  # ncp, frp 8282 - > 80 @ armbian_chanidBox
          websocket
          transparent
        }
  }
  ```
  - Add the LAN_IP; Domain; 127.0.0.1 into the trust domain list of ncp:
    
    add the domain `cloud.yourdomain.com` 
    
    `docker exec -it nextcloudpi ncp-config` 
    
#### Start docker container automatically.
    You may need the container get started automatically for cases such as interrupted. There are two ways I can find:
    - use `docker run -d --restart <flag> <container_name>` when you pull the image and build the container at the first time. [ref link](https://docs.docker.com/config/containers/start-containers-automatically/)
    - create a systemd service on host. [ref link](https://blog.container-solutions.com/running-docker-containers-with-systemd)
The example content of `/etc/systemd/system/docker.nextcloudpi.service`

```ini
[Unit]
Description=nextCloudPi Container
After=docker.service
Requires=docker.service
After=frpc.service
Requires=frpc.service     # if you use frpc for exposing the local service to public Internet

[Service]
TimeoutStartSec=30
ExecStart=/usr/bin/docker start nextcloudpi

[Install]
WantedBy=multi-user.target
```
enable this service run on boot: `sudo systemctl enable docker.nextcloudpi.service`. Note this .service file may cause fail. test it by reboot your host.

#### Stop Auto-start container:
   - `$ docker update --restart unless-stopped <container_name>`, change the `unless-stopped` into `no`
   - and modify the `/etc/systemd/system/docker.nextcloudpi.service`

### create new nextCloud user:

>  NextCloud does not support users to register themselves? Funny...

  ref: https://docs.nextcloud.com/server/13/admin_manual/configuration_user/user_configuration.html#creating-a-new-user

### Tips:

##### Different Quota options - configure?
  The options for user quotas are "Default", "Unlimited", 1GB, 5GB and 10GB. If you'd like to be able to set a 25GB, just type in your own quota in. e.g. 1TB limit just type in "1TB". [ref_link](https://www.reddit.com/r/NextCloud/comments/bf088u/different_quota_options_configure/)

##### What http server does ncp use?
  try `systemctl status apache2` in ncp container's bash
  manually stopping the daemon of apache2:
```bash
 # systemctl stop apache2
 # systemctl disable apache2
```

## What if install ncp via curl command
We can now install NextCloudPi in any Debian Buster system of any architecture with:
```bash
# curl -sSL https://raw.githubusercontent.com/nextcloud/nextcloudpi/master/install.sh | bash
```  
before running above, may need to copy the nc-app and nc-web, ref: https://github.com/nextcloud/nextcloudpi/issues/976
before running above, may need to configure the debian source.list


##

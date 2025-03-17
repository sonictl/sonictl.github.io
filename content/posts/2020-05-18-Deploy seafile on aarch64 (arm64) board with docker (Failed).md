---
layout: post
title: 'Deploy seafile on aarch64 (arm64) board with docker (Failed)'
date: 2020-05-18 22:43:00 +0800
category: from_cnblogs
slug: p20200518224300
---
#Deploy seafile on aarch64 (arm64) board with docker (Failed):
## 在Arm64或aarch64板卡上通过docker安装seafile (未成功)

> This failure may due that the image does not support the aarch64 architecture. If the host is x86 or amd64 architecture, things suppose to be better.
> 未成功启动container。问题原因可能在于docker image 不支持aarch64架构，换作 amd64 或 x86应该会好起来。

### 1. Get ready docker
- Check your system:
```bash
$ uname -s
  Linux
$ uname -m
  aarch64
$ docker --version
  Docker version 18.09.1, build 4c52b90
```
- Install Docker: [ref_this](https://www.cnblogs.com/sonictl/p/12909874.html)


### 2. Install seafile with docker:
run commands after read the whole content of this blog.
You may need to modify cmd below with reference of section3
```bash
docker run -d --name seafile \
  -e SEAFILE_SERVER_HOSTNAME=seafile.example.com \
  -e SEAFILE_ADMIN_EMAIL=me@example.com \
  -e SEAFILE_ADMIN_PASSWORD=a_very_secret_password \
  -v /opt/seafile-data:/shared \
  -p 80:80 \
  seafileltd/seafile:latest
```
e.g.  
```bash
docker run -d --name seafile \
  -e SEAFILE_SERVER_HOSTNAME=127.0.0.1 \
  -e SEAFILE_ADMIN_EMAIL=me@example.com \
  -e SEAFILE_ADMIN_PASSWORD=yourPassWord \
  -v /data/seafile/seafile-data:/shared \
  -p 8999:80 \
  seafileltd/seafile:latest
```


### 3. Option: sse SSL certificate:
 ```bash
docker run -d --name seafile \
  -e SEAFILE_SERVER_LETSENCRYPT=true \
  -e SEAFILE_SERVER_HOSTNAME=seafile.example.com \
  -e SEAFILE_ADMIN_EMAIL=me@example.com \
  -e SEAFILE_ADMIN_PASSWORD=a_very_secret_password \
  -v /opt/seafile-data:/shared \
  -p 80:80 \
  -p 443:443 \
  seafileltd/seafile:latest
 ```

### Failed with starting the container, the error in log:
 check log:
```bash
# docker logs seafile
standard_init_linux.go:207: exec user process caused "exec format error"
standard_init_linux.go:207: exec user process caused "exec format error"
```
> This may due that the image does not support the aarch64 architecture. If the host is x86 or amd64 architecture, things suppose to be better.


----

## Read More: modify seafile server configurations

The config files are under `shared/seafile/conf`. You can modify the configurations according to [Seafile manual](https://manual.seafile.com/)

After modification, you need to restart the container:



```
docker restart seafile
```

### Find logs

The seafile logs are under `shared/logs/seafile` in the docker, or `/opt/seafile-data/logs/seafile` in the server that run the docker.

The system logs are under `shared/logs/var-log`, or `/opt/seafile-data/logs/var-log` in the server that run the docker.

### Add a new Admin

Ensure the container is running, then enter this command:



```
docker exec -it seafile /opt/seafile/seafile-server-latest/reset-admin.sh
```

Enter the username and password according to the prompts. You now have a new admin account.

## Directory Structure

### `/shared`

Placeholder spot for shared volumes. You may elect to store certain persistent information outside of a container, in our case we keep various logfiles and upload directory outside. This allows you to rebuild containers easily without losing important information.

- /shared/db: This is the data directory for mysql server
- /shared/seafile: This is the directory for seafile server configuration and data.
- /shared/logs: This is the directory for logs.
  - /shared/logs/var-log: This is the directory that would be mounted as `/var/log` inside the container. For example, you can find the nginx logs in `shared/logs/var-log/nginx/`.
  - /shared/logs/seafile: This is the directory that would contain the log files of seafile server processes. For example, you can find seaf-server logs in `shared/logs/seafile/seafile.log`.
- /shared/ssl: This is directory for certificate, which does not exist by default.
- /shared/bootstrap.conf: This file does not exist by default. You can create it by your self, and write the configuration of files similar to the `samples` folder.

## Upgrading Seafile Server

TO upgrade to latest version of seafile server:



```
docker pull seafileltd/seafile:latest
docker rm -f seafile
docker run -d --name seafile \
  -e SEAFILE_SERVER_LETSENCRYPT=true \
  -e SEAFILE_SERVER_HOSTNAME=seafile.example.com \
  -e SEAFILE_ADMIN_EMAIL=me@example.com \
  -e SEAFILE_ADMIN_PASSWORD=a_very_secret_password \
  -v /opt/seafile-data:/shared \
  -p 80:80 \
  -p 443:443 \
  seafileltd/seafile:latest
```

If you are one of the early users who use the `launcher` script, you should refer to [upgrade from old format](https://github.com/haiwen/seafile-docker/blob/master/upgrade_from_old_format.md) document.

## Backup and Recovery

### Struct

We assume your seafile volumns path is in `/shared`. And you want to backup to `/backup` directory. You can create a layout similar to the following in /backup directory:



```
/backup
---- databases/  contains database backup files
---- data/  contains backups of the data directory
```

The data files to be backed up:



```
/shared/seafile/conf  ## configuration files
/shared/seafile/pro-data  ## data of es
/shared/seafile/seafile-data ## data of seafile
/shared/seafile/seahub-data ## data of seahub
```

### Backup

Steps:

1. Backup the databases;
2. Backup the seafile data directory;

[Backup Order: Database First or Data Directory First]()

- backing up Database:

  

  ```
  ## It's recommended to backup the database to a separate file each time. Don't overwrite older database backups for at least a week.
  cd /backup/databases
  docker exec -it seafile mysqldump  -uroot --opt ccnet_db > ccnet_db.sql
  docker exec -it seafile mysqldump  -uroot --opt seafile_db > seafile_db.sql
  docker exec -it seafile mysqldump  -uroot --opt seahub_db > seahub_db.sql
  ```

- Backing up Seafile library data:

  - To directly copy the whole data directory

    

    ```
    cp -R /shared/seafile /backup/data/
    cd /backup/data && rm -rf ccnet
    ```

  - Use rsync to do incremental backup

    

    ```
    rsync -az /shared/seafile /backup/data/
    cd /backup/data && rm -rf ccnet
    ```

## Recovery

- Restore the databases:

  

  ```
  cp /backup/data/ccnet_db.sql /shared/ccnet_db.sql
  cp /backup/data/seafile_db.sql /shared/seafile_db.sql
  cp /backup/data/seahub_db.sql /shared/seahub_db.sql
  docker exec -it seafile /bin/sh -c "mysql -uroot ccnet_db < /shared/ccnet_db.sql"
  docker exec -it seafile /bin/sh -c "mysql -uroot seafile_db < /shared/seafile_db.sql"
  docker exec -it seafile /bin/sh -c "mysql -uroot seahub_db < /shared/seahub_db.sql"
  ```

- Restore the seafile data:

  

  ```
  cp -R /backup/data/* /shared/seafile/
  ```

## Garbage Collection

When files are deleted, the blocks comprising those files are not immediately removed as there may be other files that reference those blocks (due to the magic of deduplication). To remove them, Seafile requires a ['garbage collection'](https://manual.seafile.com/maintain/seafile_gc.html) process to be run, which detects which blocks no longer used and purges them. (NOTE: for technical reasons, the GC process does not guarantee that *every single* orphan block will be deleted.)

The required scripts can be found in the `/scripts` folder of the docker container. To perform garbage collection, simply run `docker exec seafile /scripts/gc.sh`. For the community edition, this process will stop the seafile server, but it is a relatively quick process and the seafile server will start automatically once the process has finished. The Professional supports an online garbage collection.

## Troubleshooting

You can run docker commands like "docker logs" or "docker exec" to find errors.



```
docker logs -f seafile
# or
docker exec -it seafile bash
```
    
    
    
    
    
    ```bash
    
    ```
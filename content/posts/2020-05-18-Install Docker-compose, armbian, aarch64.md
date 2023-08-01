---
layout: post
title: 'Install Docker-compose, armbian, aarch64'
date: 2020-05-18 04:39:00 +0800
category: from_cnblogs
slug: p20200518043900
---
# Install Docker-compose, armbian, aarch64
reference:
 - [aarch64安装Docker Compose](https://www.huaweicloud.com/kunpeng/software/dockercompose.html)
 - [Docker 17.03.2-ce on Arm64 (Aarch64) Debian](https://medium.com/@danacr/docker-17-03-2-ce-on-arm64-aarch64-debian-6c281656f79)
### 1. Check your system

```bash
$ uname -s
  Linux
$ uname -m
  aarch64
$ docker --version
  Docker version 18.09.1, build 4c52b90
```

reference: https://www.huaweicloud.com/kunpeng/software/dockercompose.html

### 2. Compile 

Follow the link above and test after finished.

### 3. Test

bash commands and result for reference:

```bash
root@bbhost:/usr/local/src/docker-compose-aarch64# docker run docker-compose-aarch64-builder
root@bbhost:/usr/local/src/docker-compose-aarch64# find / -name "docker-compose-Linux-aarch64"
/var/lib/docker/volumes/7ea3070ed7150ec80bd98ee7a09875cd891b98c2f570a656685dbd231dbb31be/_data/docker-compose-Linux-aarch64
/var/lib/docker/overlay2/83f1b21fb322a2f5b3eda68d2ae785c7fff1eaa14d36b6bcbd3df7ea89775501/diff/build/dockercompose/docker-compose-Linux-aarch64
root@bbhost:/usr/local/src/docker-compose-aarch64# /var/lib/docker/volumes/7ea3070ed7150ec80bd98ee7a09875cd891b98c2f570a656685dbd231dbb31be/_data/docker-compose-Linux-aarch64 --version
docker-compose version 1.22.0, build e20d808e
```
you may create a symlink for better convenience:
```bash
# ln -s /var/lib/docker/volumes/7ea3070ed7150ec80bd98ee7a09875cd891b98c2f570a656685dbd231dbb31be/_data/docker-compose-Linux-aarch64 /usr/local/bin/docker-compose
```
:)




**Reference:**

----

## 软件介绍



Docker Compose是Docker编排服务的一部分，Compose可以让用户在集群中部署分布式应用。Docker Compose属于一个“应用层”的服务，用户可以定义哪个容器组运行哪个应用，它支持动态改变应用，并在需要时扩展。

建议使用版本为“Docker Compose-1.22.0”。



**环境要求**

**云服务器要求**

本文以云服务器KC1实例测试，云服务器配置如表1-1所示。

表1-1云服务器配置

| 项目 | 说明                         |
| ---- | ---------------------------- |
| 规格 | kc1.large.2 \| 2vCPUs \| 4GB |
| 磁盘 | 系统盘：高IO（40GB）         |



**操作系统要求**

操作系统要求如表1-2所示。

表1-2操作系统要求

| 项目   | 说明       | 下载地址             |
| ------ | ---------- | -------------------- |
| CentOS | 7.6        | 在公共镜像中已提供。 |
| Kernel | 4.14.0-115 | 在公共镜像中已提供。 |



\1.   配置安装环境

1)  安装wget和openjdk。

**yum install java-1.8.0-openjdk java-1.8.0-openjdk-devel wget -y**

2)  安装python36。

**yum install python36 -y**

3)  安装docker。

**yum install docker**

4)  启动docker。

**systemctl start docker**

5)  检查docker是否安装成功，显示如下表示安装成功

**docker --version**

Docker version 1.13.1, build 7f2769b/1.13.1

----结束

\2.   获取软件包

获取“Docker Compose-1.22.0”安装包。

**cd /usr/local/src**

**git clone https://github.com/ubiquiti/docker-compose-aarch64.git**

\3.   安装

1)  进入docker-compose源文件目录。

**cd /usr/local/src/docker-compose-aarch64**

2)  配置Dockerfile。

**vi Dockerfile**

注释掉RUN [ "cross-build-start" ]，即在之前加入‘#’后保存退出。

\# Dockerfile to build docker-compose for aarch64
FROM arm64v8/python:3.6.5-stretch
 
\# Add env
ENV LANG C.UTF-8
 
\# Enable cross-build for aarch64
COPY ./vendor/qemu-bin /usr/bin/
**#RUN [ "cross-build-start" ]**

3)  安装docker-compose。

**cd /usr/local/src/docker-compose-aarch64**

**docker build . -t docker-compose-aarch64-builder**

----结束

\4.   运行和验证

1)  运行docker-compose容器。

**docker run docker-compose-aarch64-builder**

2)  找到生成的“docker-compose”可执行程序并执行。

**find / -name "docker-compose-Linux-aarch64"**

[root@ecs-teukh-1 docker-compose-aarch64]# find / -name "docker-compose-Linux-aarch64"
/var/lib/docker/overlay2/1d8081e2d4b5958a1eccbaf76e949219c260d89236315b48cf0bfa95e076c1da/diff/build/dockercompose/docker-compose-Linux-aarch64
/var/lib/docker/overlay2/1d8081e2d4b5958a1eccbaf76e949219c260d89236315b48cf0bfa95e076c1da/diff/build/docker-compose-Linux-aarch64
/var/lib/docker/volumes/9d6624e6fc53d37221774fed9c64cf1a4ce64319a221e1069c70b4c88df7be40/_data/docker-compose-Linux-aarch64

可以看到有三个目录存放了生成的“docker-compose-Linux-aarch64”可执行程序。

3)  进入任意一个目录。

**cd /var/lib/docker/overlay2/1d8081e2d4b5958a1eccbaf76e949219c260d89236315b48cf0bfa95e076c1da/diff/build/dockercompose/**

**./docker-compose-Linux-aarch64 --version**

[root@ecs-teukh-1 build]# ./docker-compose-Linux-aarch64 --version
docker-compose version 1.22.0, build e20d808e

显示类似上述，表明docker-compose安装成功。

----结束
---
title : "Docker离线安装"
date : "2018-05-03T20:54:29+08:00"
tags : [
  "Docker",
  "centos7"
]
draft : false
---
# 安装docker与docker-compose

> 本文主要解决在centos7服务器无法访问外网的时候安装docker的问题

## 在可访问外网的电脑上下载docker所需文件

- [docker-ce-selinux-17.03.2.ce-1.el7.centos.noarch.rpm](https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-selinux-17.03.2.ce-1.el7.centos.noarch.rpm)
- [docker-ce-17.03.2.ce-1.el7.centos.x86_64.rpm](https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-17.03.2.ce-1.el7.centos.x86_64.rpm)

## 将下载的两个文件传送到centos7服务器上

```bash
scp docker-ce-selinux-17.03.2.ce-1.el7.centos.noarch.rpm <用户名>@<服务器ip或者域名>:<存放路径>
scp docker-ce-17.03.2.ce-1.el7.centos.x86_64.rpm <用户名>@<服务器ip或者域名>:<存放路径>
```

## 使用`yum install`先后执行两个文件

```bash
# 依次执行
yum install docker-ce-selinux-17.03.2.ce-1.el7.centos.noarch.rpm
yum install docker-ce-17.03.2.ce-1.el7.centos.x86_64.rpm
```

## 启动Docker服务

```bash
sudo systemctl start docker
```

## 去掉执行docker必须sudo

- 添加docker用户组`$ sudo groupadd docker`
- 将系统用户加入`$ sudo gpasswd -a ${USER} docke`

## docker-compose安装

- 在能连接外网的linux下下载docker-compose

  ```bash
  sudo curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
  ```
- 用scp将docker-compose文件传送到无法连接外网的linux上

  ```bash
  scp /usr/local/bin/docker-compose <user>@<ip>:/home/<user>/bin/
  ```
- ssh进入无法连接外网的服务器

  ```bash
  ssh <user>@<ip>
  ```
- 给用户添加执行权限

  ```bash
  sudo chmod +x /usr/local/bin/docker-compose
  ```
- 测试是否成功

  ```bash
  $ docker-compose --version
  # 成功允许会有版本信息输出
  ```

---
title : "Docker+centos7+jira7.3.6+postgre Sql破解搭建"
date : "2018-05-03T22:19:24+08:00"
description : "Docker+centos7+jira7.3.6+postgre Sql搭建"
tags : [
  "Docker",
  "centos",
  "JIRA",
  "postgresql"
]
draft : false
---
# 依次操作

## 准备必要文件

百度盘<https://pan.baidu.com/s/1brXZBvfhUCZA0RBGOsMP0w>

### jar包准备

- jira破解所需：_atlassian-extras-3.2.jar_

### docker镜像准备

```bash
# jira镜像
docker pull cptactionhank/atlassian-jira:7.3.6
# postgresql镜像
docker pull postgres:9.3
```

## 运行镜像

### 创建文件夹jira，并进入文件夹目录

```bash
mkdir jira && cd jira
```

### 创建jira_dockerfile文件，并且编辑保存

```bash
touch jira_dockerfile
vim jira_dockerfile
```

#### 添加内容

```docker
FROM cptactionhank/atlassian-jira:7.3.6

COPY atlassian-extras-3.2.jar  /opt/atlassian/jira/atlassian-jira/WEB-INF/lib/
```

### 创建docker-compose.yml文件，编辑保存

```bash
touch docker-compose.yml
vim docker-compose.yml
```

#### 添加内容

```yaml
version: '3'
services:

jira:
    build:
    context: .
    dockerfile: jira-dockerfile
    ports:
    - "9090:8080"
    links:
    - db
    depends_on:
    - db

db:
    image: postgres:9.3
    ports:
    - "5432:5432"
    environment:
    POSTGRES_PASSWORD: rootroot
```

### 使用docker-compose执行

```bash
docker-compose build
docker-compose up -d
```

### 查看镜像运行状态

```bash
docker ps
```

看到cptactionhank/atlassian-jira:6.3.6与postgres:9.3镜像正在运行
![ps](http://ae01.alicdn.com/kf/HTB1Jb5FpQ9WBuNjSspe761z5VXat.png)

### 连接到postgresql数据库

> 注意：服务器防火墙需要开放9090与5432端口

- 账号：**postgres**
- 密码：**rootroot**
- 端口：**5432**
- 域名：&lt;服务器的ip地址或者对应域名>

连接后创建数据库

```sql
CREATE DATABASE jiradb WITH ENCODING='UTF8';
```

## 安装jira

> 注意：第一次进入的时候可能要等几分钟，具体时间与你服务器配置有关

### 1. 在浏览器输入 **&lt;服务器ip或者对应域名>:9090** ，进入下图，点击 _language_  ， 选择 _中文_ ，选择 _我将设置它自己_ ：

![home](http://ae01.alicdn.com/kf/HTB1rG3DhBmWBuNkSndV763sApXa8.png)

### 2. 按照图上输入信息，密码为 _rootroot_ ，点击下一步：

点击Test Connection，成功就会返回： _The database connection test was successful._
![db](http://ae01.alicdn.com/kf/HTB1ADYQeZj_B1NjSZFH761DWpXah.png)

### 3. 分别填写信息如下图：

![writedb](http://ae01.alicdn.com/kf/HTB1p1FWe.OWBKNjSZKz760fWFXac.png)

### 4. 填好信息，点击下一步，如下图：

  ![appinfo](http://ae01.alicdn.com/kf/HTB15HB_pFmWBuNjSspd762ugXXaT.png)

- 程序标题：指定是应用名字，可以随意写
- 模式：模式有两种
    - 开放：指的是可以公开注册jira账号，并且通过管理员添加jira账号
    - 私有：指的是只能通过管理员添加jira账号
- 基本URL：指的是jira访问地址 **http&#x3A;//&lt;服务器ip或者域名>:9090** ，默认会帮你填上，无需修改

### 5. 点击 _生成JIRA试用许可证_ ：

![custom](http://ae01.alicdn.com/kf/HTB1BNsXX8gXBuNjt_hN761EiFXaR.png)

- 登录altassian
    ![login](http://ae01.alicdn.com/kf/HTB1iHAkXVqZBuNjt_jq761mzpXa0.png)
- 按照下图，填写信息（Organisation可以随意写）
    ![info](http://ae01.alicdn.com/kf/HTB1PVRjp29TBuNjy0Fc762eiFXai.png)
- 点击Yes
    ![ok](http://ae01.alicdn.com/kf/HTB1A0RZpY1YBuNjSszh763UsFXaS.png)
- 点击下一步
    ![next](http://ae01.alicdn.com/kf/HTB1vve1e9MmBKNjSZTE761sKpXaO.png)

### 6. 点击完 _下一步_ 自动跳到设置超级管理员界面，填写必要信息，点击 _下一步_

![writeadmin](http://ae01.alicdn.com/kf/HTB112J0p1uSBuNjSszi762q8pXay.png)

### 7. 点击 _完成_

![fins](http://ae01.alicdn.com/kf/HTB1Iravp4GYBuNjy0Fn7605lpXau.png)

### 8. 点击 _Continue_ 完成创建

![qg](http://ae01.alicdn.com/kf/HTB1ucFkp29TBuNjy0Fc762eiFXaY.png)

### 9. 可以上传你的头像，点击 _下一步_

![avatar](http://ae01.alicdn.com/kf/HTB1zJ9dp1uSBuNjy1Xc763YjFXaI.png)

### 10. 创建一个项目

- 点击 _创建示例项目_
    ![create](http://ae01.alicdn.com/kf/HTB1Ak0BpYuWBuNjSszg7628jVXaC.png)
- 点击 _下一步_
    ![next](http://ae01.alicdn.com/kf/HTB18s3pX1UXBuNjt_XB760eDXXaE.png)
- 点击 _提交_
    ![submit](http://ae01.alicdn.com/kf/HTB1aRMDhvuSBuNkHFqD760fhVXas.png)
- 完成创建
    ![project](http://ae01.alicdn.com/kf/HTB1OVYjp9tYBeNjSspa761OOFXaA.png)

## 查看是否破解

### 点击 _应用程序_

![len-check](http://ae01.alicdn.com/kf/HTB1ADEmhyCYBuNkSnaV763MsVXaw.png)

### 输入管理员密码

![pass](http://ae01.alicdn.com/kf/HTB1ksplp29TBuNjy0Fc762eiFXar.png)

### 查看，显示2033年2月08日过期，破解成功

![suc](http://ae01.alicdn.com/kf/HTB1Qg49p25TBuNjSspm761DRVXa6.png)

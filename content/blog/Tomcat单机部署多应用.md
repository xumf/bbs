---
title: "Tomcat单机部署多应用（Mac/Linux）"
date: 2018-04-22T13:16:53+08:00
tags: ["tomcat"]
---
# 按照以下步骤依次执行：

> `${tomcat}`tomcat的根目录

## 1.  修改`/etc/profile`增加tomcat环境变量

    export CATALINA_BASE=/Users/xumf/tomcat1
    export CATALINA_HOME=/Users/xumf/tomcat1
    export TOMCAT_HOME=/Users/xumf/tomcat1
    export CATALINA_2_BASE=/Users/xumf/tomcat2
    export CATALINA_2_HOME=/Users/xumf/tomcat2
    export TOMCAT_2_HOME=/Users/xumf/tomcat2

## 2.  保存退出，输入esc然后`:wq`

## 3.  执行

```bash
source /etc/profile
```

## 4.  第一个tomcat不变，打开第二个tomcat目录bin下catalina.sh

```bash
vim ${tomcat}/bin/catalina.sh
```

## 5.  找到# OS specific support. $var _must_ be set to either true or false.在这行下面编辑，新增配置，保存退出

    export CATALINA_BASE=$CATALINA_2_BASE
    export CATALINA_HOME=$CATALINA_2_HOME

## 6.  打开第二个tomcat的conf目录下server.xml

```bash
vim ${tomcat}/conf/server.xml
```

- 8005改为9005
    ![0AE3D7B7-F96A-4FAE-A13A-94B836BCD4DF](https://i.imgur.com/rpuceXa.png)
- 8080改为9080
    ![05F4A861-6CD4-453C-B12F-3F83ACA24D35](https://i.imgur.com/IiW7hgv.png)
- 8009改为9009
    ![8359A2A1-9443-4829-841F-70100348C9C9](https://i.imgur.com/1MGeszj.png)

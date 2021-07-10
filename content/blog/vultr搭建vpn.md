---
title : "Vultr搭建vpn"
date : "2018-04-30T22:39:17+08:00"
description : "Vultr+CentOS用l2tp协议搭建vpn"
tags : ["Vultr","vpn","l2tp"]
---

## 1. 注册[vultr](https://www.vultr.com/)账号

  ![account](https://s9.postimg.cc/fn6jn7qy7/accout.png)

## 2. 登录完成后进入[个人中心](https://my.vultr.com/)

  ![personal](http://ae01.alicdn.com/kf/HTB1CMRmpeuSBuNjy1Xc763YjFXa7.png)

## 3. 充钱，点击菜单右侧Billing

  ![Billing](http://ae01.alicdn.com/kf/HTB1hFGCeTCWBKNjSZFt761C3FXad.png)

## 4. 选择支付方式，推荐使用支付宝（vultr一次最少要充值10美元，大概63块人民币）

- Credit Card 信用卡
- Paypal [贝宝](https://www.paypal.com)
- Bitcoin [比特币](https://bitcoin.org/en/)
- Gift Code 优惠码

## 5. 冲完钱后，开始购买服务器，点击菜单的Servers，然后点击加号

  ![Servers](http://ae01.alicdn.com/kf/HTB1tvkQoVuWBuNjSszb763S7FXaE.png)

## 6. 选择购买服务器参数

  - 选择服务地区（本人选择的是日本的，vpn速度非常棒）
      ![location](http://ae01.alicdn.com/kf/HTB1ToPxgOCYBuNkSnaV763MsVXaF.png)
  - 选择服务器OS（本人选择的CentOS 7）
      ![os](http://ae01.alicdn.com/kf/HTB1fcympmtYBeNjSspk762U8VXaB.png)
  - 选择服务器配置（一般做vpn的话，选$2.50/mo，就是2.5美元一个就可以，但是有时候会没有需要抢）
      ![size](http://ae01.alicdn.com/kf/HTB1leHCgIyYBuNkSnfo763WgVXao.png)
  - 如下图的参数默认即可
      ![params](http://ae01.alicdn.com/kf/HTB1exU1o1SSBuNjy0Fl762BpVXam.png)
    \* 如下图红框中可以填写你的域名，最后点击Deploy Now完成购买
      ![host](http://ae01.alicdn.com/kf/HTB1hsNiePQnBKNjSZSg760HGXXaT.png)

## 7. 购买完成后，在Servers菜单下就会有vpc的实例，下图中红圈里面的是你vpc服务器的外网ip，

  ![instance](http://ae01.alicdn.com/kf/HTB1DqvwgOCYBuNkHFCc763HtVXac.png)

## 8. 单击上图红色矩形区域进入服务器管理界面

> 下面可以看到root用户的密码

![info](http://ae01.alicdn.com/kf/HTB1uaw4oYSYBuNjSspi760NzpXaq.png)

## 9. 进入服务器，安装TCP BBR提速工具

- 通过ssh进入服务器，密码可以通过上一个步获取

```bash
$ ssh root@<你服务器的ip>
```
-   下载bbr

```bash
$ wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
```
-   添加执行权限

```bash
$ chmod +x bbr.sh
```
-   执行bbr.sh

```bash
$ ./bbr.sh
```
-   执行后会有此 **Press any key start ... or Press Ctrl+C to cancel** 提示点击回车就可以
-   最有会有此 **Info: The system  needs to be restart. Do you want to reboot? [y/n]** 提示，要求重启，输入y，重启电脑
-   输入命令测试是否安装成功

```bash
$ sysctl net.ipv4.tcp_available_congestion_control
# 返回net.ipv4.tcp_available_congestion_control = reno cubic bbr
$ lsmod | grep bbr
# 返回tcp_bbr                20480  9
```

## 10. 安装l2tp vpn

- 获取vpn

```bash
$ wget --no-check-certificate https://raw.githubusercontent.com/teddysun/across/master/l2tp.sh
```

- 给l2tp.sh添加执行权限

> 记住下面步骤的 **_密钥_** ，客户端连接vpn的时候要用

```bash
# 执行后会提示叫你输入用户名，密码，密钥的提示，你可以自己填写也可以直接回车选择默认。
$ chmod +x l2tp.sh
```

- 重启ipsec和xl2tpd服务

```bash
$ systemctl restart ipsec
$ systemctl restart xl2tpd
```

- 验证是否成功

```bash
$ ipsec verify
```

![success](http://ae01.alicdn.com/kf/HTB1P31cpf1TBuNjy0Fj761jyXXav.png)

- 使用 _l2tp_ 命令

    - `l2tp -a`添加vpn用户

        ![add-user](http://ae01.alicdn.com/kf/HTB1FqRppXGWBuNjy0Fb7624sXXaF.png)

    - `l2tp -l`查看所有vpn用户与用户的密码，可以看到新添加的用户 _zhangsan_

        ![list-user](http://ae01.alicdn.com/kf/HTB1zRiTeOMnBKNjSZFz763_qVXaP.png)

    - `l2tp -m`修改vpn用户密码

        ![mod-user](http://ae01.alicdn.com/kf/HTB1eFNqpXGWBuNjy0Fb7624sXXa0.png)

    - `l2tp -d`删除vpn用户

        ![del-user](http://ae01.alicdn.com/kf/HTB14nCweRjTBKNjSZFD762VgVXaF.png)

    - `l2tp -h`命令帮助文档


## 11.  Mac OS客户端连接vpn

- 依次进入 _系统偏好设置_ > _网络_，点击加号
    ![add](http://ae01.alicdn.com/kf/HTB12DOSeFooBKNjSZFP760a2XXaZ.png)

    - 接口：选择vpn
    - vpn类型：选择L2TP
    - 服务名称：可以随意填

- 点击创建，选择刚创建的vpn，填写参数
    ![setting](http://ae01.alicdn.com/kf/HTB1VTT9gRyWBuNkSmFP760guVXaz.png)

    - 服务器地址：填写你购买vurtl的ip
    - 账户名称：填写刚刚l2tp创建用户（例：zhangsan)

- 点击鉴定设置，设置完成后点按钮 **好**
    ![two-step](http://ae01.alicdn.com/kf/HTB1mW1WeScqBKNjSZFg760_kXXaT.png)

    - 密码：填写vpn账户的密码
    - 共享密钥：填写步骤10中的密钥

- 点击 **_连接_** 完成vpn连接

## 12. Windows 7 系统连接vpn

参考[https://jingyan.baidu.com/article/455a99508833cca166277831.html](https://jingyan.baidu.com/article/455a99508833cca166277831.html)

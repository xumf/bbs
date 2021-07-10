---
title: "Centos Download Rabbitmq"
date: 2018-04-22T22:21:04+08:00
tags: ["Centos", "Rabbitmq"]
draft: false
---

1. 到erlang官网下载[erlang](http://www.erlang.org/downloads/20.1)

    ![3DDD02DB-A983-471B-8A8A-5DCE15F1969B](http://ae01.alicdn.com/kf/HTB1aydInHSYBuNjSspi760NzpXau.png)

2. 到rabbitMQ官网下载[mq](http://www.rabbitmq.com/install-generic-unix.html)

    ![3DDD02DB-A983-471B-8A8A-5DCE15F1969B](http://ae01.alicdn.com/kf/HTB11GX5nFGWBuNjy0Fb7624sXXaO.png)

3. 安装依赖包

    ```bash
    yum install -y gcc gcc-c++ unixODBC-devel  openssl-devel ncurses-devel
    ```

4. 将下载好的两个包传送到centos服务器上

    ```bash
    scp otp_src_20.1.tar.gz root@www.xumingfa.com:/usr/src/
    scp rabbitmq-server-generic-unix-3.7.4.tar.xz root@www.xumingfa.com:/usr/src/
    ```

5. 解压erlang包

    ```bash
    tar xf otp_src_20.1.tar.gz
    ```

6. 切换到目录otp_src_20.1

    ```bash
    cd otp_src_20.1
    ```

7. 配置.configure（安装到 `/usr/local/erlang20` 目录 , 忽略 javac 编译器）

    ```bash
    ./configure --prefix=/usr/local/erlang20 --without-javac
    ```

8. 编译

    ```bash
    make -j 4
    make install
    ```

9. 解压mq包

    ```bash
    cd ..
    xz -d rabbitmq-server-generic-unix-3.7.4.tar.xz
    tar xf rabbitmq-server-generic-unix-3.7.4.tar
    ```

10. 安装依赖

    ```bash
    yum install python -y
    yum install xmlto -y
    pip install simplejson
    ```

11. 将mq放到`/usr/local/rabbitmq`

    ```bash
    mv rabbitmq_server-3.7.4 /usr/local/rabbitmq
    cd /usr/local/rabbitmq/sbin/
    ```

12. 配置环境变量

    ```bash
    vim /etc/profile
    // 在profile文件后面追加
    # erlang
    PATH=$PATH:/usr/local/erlang20/bin
    export PATH
    # rabbitmq
    PATH=$PATH:/usr/local/rabbitmq/sbin
    export PATH
    # 更新
    source /etc/profile
    ```

13. 启动rabbitmq服务

    ```bash
    rabbitmq-server
    ```

14. 关闭rabbitmq服务

    ```bash
    rabbitmqctl stop
    ```

15. 设置远程访问（默认guest只允许本地访问）

    ```bash
    cd /usr/local/rabbitmq/
    vim ./etc/rabbitmq/rabbitmq.config
    # 添加
    [{rabbit, [{loopback_users, []}]}].
    # wq保存退出
    # 重启rabbitmq服务
    rabbitmqctl stop
    rabbitmq-server
    ```

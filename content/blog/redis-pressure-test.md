---
title: "Redis压测"
date: 2018-04-22T15:40:53+08:00
tags: ["redis"]
draft: false
---

```bash
#（100个并发连接，100000个请求）
$ redis-benchmark -h 127.0.0.1 -p 6379 -c 100 -n 100000
```

```bash
#（-d 存取大小为100字节的数据包, -q 简单输出）
$ redis-benchmark -h 127.0.0.1 -p 6379 -q -d 100
```

```bash
#（-t 测试的类别）
$ redis-benchmark -t set,lpush -q -n 100000
```

```bash
#（script local 指的是就执行script local跟着的命令）
redis-benchmark -n 100000 -q script load "redis.call(‘set’,’foo’,’bar’)"
```

---
title: "Redis命令"
date: 2018-04-19T21:09:25+08:00
tags: ["redis"]
---

## redis的启动命令

- 启动客户端

    ```bash
      $ redis-cli
    ```

- 关闭客户端

    ```bash
    $ redis-cli shutdown
    ```

- 指定端口启动

    ```bash
    $ redis-cli -p 6380
    ```

- 指定端口关闭

    ```bash
    $ redis-cli -p 6380 shutdown
    ```

- 指定ip启动

    ```bash
    $ redis-cli -h 127.0.0.1
    ```

- 指定ip关闭

    ```bash
    $ redis-cli -h 127.0.0.1 shutdown
    ```

- 指定密码启动

    ```bash
    $ redis-cli -a 123456
    ```

## redis基础命令

- 清除当前数据库所有的键

    ```bash
    $ flushdb
    ```

- 清除全部数据库所有的键

    ```bash
    $ flushall
    ```

- 测试数据库链接是否成功

    ```bash
    $ ping
    ```

- 当前数据库key的数量

    ```bash
    $ dbsize
    ```

- 切换数据库

    ```bash
    $ select 数据库编号
    ```

- 查看当前数据库所有的key

    ```bash
    $ keys *
    ```

- 持久化

    ```bash
    $ save
    ```

-   退出

    ```bash
    $ quit
    ```

## redis键基础命令

- 添加一个key

    ```bash
    $ set 键 值
    ```

- 删除一个key

    ```bash
    $ del 键
    ```

- 判断键是否存在（返回1为存在，0位不存在）

    ```bash
    $ exists 键
    ```

- 查询键的剩余生存时间（-1为永远，不存在的key为-2，以秒为单位）

    ```bash
    $ ttl 键
    ```

- 设置键的有效时间（10ms）

    ```bash
    $ expire 键 10
    ```

- 查看键的类型

    ```bash
    $ type 键
    ```

-   设置一个hash值（hset 键 字段 值）

    ```bash
    $ hset 键 字段 值
    ```

-   随机键

    ```bash
    $ randomkey
    ```

-   修改键的名字，无论key是否存在

    ```bash
    $ rename 键 新键名
    ```

-   修改键的名字，如果key存在就不改变

    ```bash
    $ renamenx 键 新键名
    ```

-   设置键并且设置有效时间（秒为单位）（setex 键 有效时间 值）

    ```bash
    $ setex 键 100 值
    ```

-   设置键并且设置有效时间（毫秒为单位）（psetex 键 有效时间 值）

    ```bash
    $ psetex 键 10000 值
    ```

-   通过键获取对应的值

    ```bash
    $ get 键
    ```

-   获取键对应值得一段闭区间值（getrange 键 起始下标 结束下标）

    ```bash
    $ getrange 键 0 1
    ```

-   设置键的时候，如果键已存在，设置成功后返回旧的值（getset 键 值）

    ```bash
    $ getset 键 值
    ```

-   批量设置多个键值（mset 键 值 键 值 键 值 …）

    ```bash
    $ mset 键 值 键 值 键 值
    ```

-   批量获取读个键的值（mget 键 键 键  …）

    ```bash
    $ mget 键1 键2 键3
    ```

-   设置键，如果已存在则不做操作（setnx 键 值）

    ```bash
    $ setnx 键 值
    ```

-   获取键对应值得长度

    ```bash
    $ strlen 键
    ```

-   批量设置键值，如果有其中一个已经存在，则全部设置失败）

    ```bash
    $ msetnx 键 值 键 值 键 值
    ```

-   给键对应值是integer类型的加一（键对应的值要为数值类型）

    ```bash
    $ incr 键
    ```

-   给键对应值是integer类型的加一定的增量（incrby 键 增量值）

    ```bash
    $ incrby 键 100
    ```

-   给键对应值是integer类型的减一

    ```bash
    $ decr 键
    ```

-   给键对应值是integer类型的加一定的减量（decrby 键 减量值）

    ```bash
    $ decrby 键 100
    ```

-   给键对应值追加内容

    ```bash
    $ append 键 追加的值
    ```

## hash类型的操作

-   查看哈希类型的键，字段是否存在（hexists 键 字段）

    ```bash
    $ hexists 键 哈希类型的键
    ```

-   获取哈希类型的所有的建，以及键所有的字段的值（hgetall 键）

    ```bash
    $ hgetall 键
    ```

-   获取哈希类型的键所有的字段（hkeys 键）

    ```bash
    $ hkeys 键
    ```

-   获取哈希类型的键所有的字段的值（hvals 键）

    ```bash
    $ hvals 键
    ```

-   获取哈希类型的键字段数量（hlen 键）

    ```bash
    $ hlen 键
    ```

-   批量获取键对应的值（hmget 键 字段 字段 字段 …）

    ```bash
    $ hmget 键 哈希类型的键1 哈希类型的键2 哈希类型的键3
    ```

-   批量设置键中的字段对应值（hmset 键 字段 值 字段 值 字段 值 …）

    ```bash
    $ hmset 键 哈希类型的键1 值1 哈希类型的键2 值2 哈希类型的键3 值3
    ```

-   批量设置键中的字段对应值，如果对应字段已经存在则失败（hmsetnx 键 字段 值 字段 值 字段 值 …）

    ```bash
    $ hmsetnx 键 哈希类型的键1 值1 哈希类型的键2 值2 哈希类型的键3 值3
    ```

## list类型的操作

-   设置集合类型（lpush 键 值 值 值 值 …）

    ```bash
    $ lpush 键 值1 值2 值3 值4 值5 值6
    ```

-   查看集合的长度

    ```bash
    $ llen 键
    ```

-   拿集合的一个范围（lrange 键 起始下标 结束下标）

    ```bash
    $ lrange 键 0 2
    ```

-   设置集合下标的对应值（lset 键 下标 值）

    ```bash
    $ lset 键 0 89
    ```

-   获取集合某个下标的值（lindex 键 下标）

    ```bash
    $ lindex 键 0
    ```

-   移除集合第一个元素

    ```bash
    $ lpop 键
    ```

-   移除集合最后一个元素

    ```bash
    $ rpop 键
    ```

    ## （无序）set类型的操作

-   添加集合

    ```bash
    $ sadd 键 值1 值2 值3
    ```

-   查看集合的所有元素

    ```bash
    $ smembers 键
    ```

-   查看两个集合的差集（sdiff 集合1 集合2 返回集合1中集合2没有的元素）

    ```bash
    $ sdiff 键1 键2
    ```

-   查看两个集合的交集

    ```bash
    $ sinter 键1 键2
    ```

-   查看两个集合的并集

    ```bash
    $ sunion 键1 键2
    ```

-   查看集合中的随机元素（srandmember 集合 一次获取元素的数量）

    ```bash
    $ srandmember 键 2
    ```

-   查看某个值是否为集合的元素（sismember 集合 元素）

    ```bash
    $ sismember 键 元素
    ```

-   移除集合中一个或者多个元素（srem 集合 元素 元素 …）

    ```bash
    $ srem 键 元素
    ```

-   移除并返回一个元素

    ```bash
    $ spop 键
    ```

    ## （sortedset）有序集合类型操作

-   添加集合（zadd 键 值 集合中的键 值 集合中的键 值 集合中的键 ...）

    ```bash
    $ zadd 键 值 集合中的键 值 集合中的键 值 集合中的键
    ```

-   查看集合长度

    ```bash
    $ zcard 键
    ```

-   获取对应元素的值（zscore 集合 字段）

    ```bash
    $ zscore 键 集合中的键
    ```

-   查看符合区间条件的元素数量（zcount 集合 开始 结束）

    ```bash
    $ zcount 键 0 300
    ```

-   查看元素的索引

    ```bash
    $ zrank 集合 集合中的键
    ```

-   给某个元素增加一定的增量（zincrby sortedset 增量值 元素）

    ```bash
    $ zincrby 键 1000 集合中的键
    ```

-   获取集合中范围的元素值（zrange 集合 开始 结束）

    ```bash
    $ zrange 键 0 100
    ```

-   获取集合中范围的元素与元素值（zrange 集合 开始 结束 withscores）

    ```bash
    $ zrange sortedset 0 100 withscores
    ```

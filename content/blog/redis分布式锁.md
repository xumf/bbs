---
title: "Redis分布式锁"
date: 2018-04-22T15:43:54+08:00
tags: ["redis"]
draft: false
---
## 代码实现分布式锁

### springboot依赖
```xml
<!-- StringRedisTemplate需要 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!-- @slf4j注解使用 -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.16.6</version>
</dependency>
```

### 实现逻辑

> 方法：返回值true代表获取锁成功，返回值false则相反

> 方法：参数value=当前时间+超时时间

> 方法：参数key为redis的键

1. 判断key是否已存在，不存在就新增，返回true，已存在不改变原有key的value，返回false
2. 获取key对应的cuurentValue（设value时间+超时时间）
3. 判断锁是否超过有效期， _是否有效_ = _有效期（value）_ < _系统当前时间_
4. 如果没有过期证明锁还在被使用，直接返回false，如果已经过期，说明锁还在被使用，并且超过了有效期，所以执行第五步
5. 通过 __getAndSet__ 先返回key的值，并且给key设置新的值
6. 判断旧的值与过了有效期的值是否相等，如果等于就返回true，否则返回false

[^_^]:
    ```mermaid
    graph TD
    A(开始) --> B{"[setIfAbsent]判断key是否在，不存在就新增"}
    B -->|true| J
    B -->|false| D[get获取当前k的currentValue]
    D --> E{判断锁是否已经过期}
    E -->|true| G["[getAndSet]先获取原来k的oldValue，后给key赋予新的value"]
    E -->|false| K
    G --> I{判断currentValue是否等于oldValue}
    I -->|true| J[返回true]
    I -->|false| K[返回false]
    J --> L
    K --> L(结束)
    ```

![flow](https://s7.postimg.cc/faep2jybf/flow.png)


### 代码

```java
package com.xumf.service;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;

@Component
@Slf4j
public class RedisLock {

    @Autowired
    private StringRedisTemplate redisTemplate;

    /**
     * 加锁
     * @param key
     * @param value 当前时间+超时时间
     * @return
     */
    public boolean lock(String key, String value) {
        if (redisTemplate.opsForValue().setIfAbsent(key, value)) {
            return true;
        }

        String currentValue = redisTemplate.opsForValue().get(key);
        // 如果锁过期
        if (!StringUtils.isEmpty(currentValue) && Long.parseLong(currentValue) < System.currentTimeMillis()) {
            String oldValue = redisTemplate.opsForValue().getAndSet(key, value);
            if (!StringUtils.isEmpty(oldValue) && oldValue.equals(currentValue)) {
                return true;
            }
        }
        return false;
    }

    /**
     * 解锁
     * @param key
     * @param value 当前时间+超时时间
     */
    public void unlock(String key, String value) {
        try {
            String currentValue = redisTemplate.opsForValue().get(key);
            if (!StringUtils.isEmpty(currentValue) && currentValue.equals(value)) {
                redisTemplate.opsForValue().getOperations().delete(key);
            }
        } catch (Exception e) {
            log.error("解锁异常，{}", e);
        }
    }
}
```

## redisson框架实现

### maven依赖
```xml

<!-- JDK 1.8+ 依赖 -->
<dependency>
   <groupId>org.redisson</groupId>
   <artifactId>redisson</artifactId>
   <version>3.6.5</version>
</dependency>

<!-- JDK 1.6+ 依赖 -->
<dependency>
   <groupId>org.redisson</groupId>
   <artifactId>redisson</artifactId>
   <version>2.11.5</version>
</dependency><!--
```

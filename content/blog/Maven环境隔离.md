---
title: "Maven环境隔离"
date: 2018-04-22T13:42:22+08:00
tags: ["maven"]
---

### 1. 在pom.xml的build标签中添加

```xml
<build>
  <resources>
      <resource>
           <!-- 配置文件夹  -->
          <directory>src/main/resources.${deploy.type}</directory>
          <excludes>
                   <!-- 打包时忽略的文件  -->
              <exclude>.jsp</exclude>
          </excludes>
      </resource>
       <!-- 公共资源文件文件夹  -->
      <resource>
          <directory>src/main/resources</directory>
      </resource>
  </resources>
</build>
```

### 2. 在pom.xml的build标签下添加

```xml
<profiles>
    <profile>
        <!-- 开发环境  -->
        <id>dev</id>
        <activation>
             <!-- 默认环境  -->
            <activeByDefault>true</activeByDefault>
        </activation>
        <properties>
            <deploy.type>dev</deploy.type>
        </properties>
    </profile>
    <profile>
         <!-- 测试环境  -->
        <id>beta</id>
        <properties>
            <deploy.type>beta</deploy.type>
        </properties>
    </profile>
    <profile>
         <!-- 生产环境  -->
        <id>prod</id>
        <properties>
            <deploy.type>prod</deploy.type>
        </properties>
    </profile>
</profiles>
```

### 3. 添加资源文件夹

* resource：共享配置
* resource.beta：测试环境配置
* resource.dev：开发环境配置
* resource.prod：生产环境配置

![96FDADDB-0500-43C5-A132-085648B3E4C1](https://i.imgur.com/PPppeP5.png)

### 4. 打包

  > \-P后面对应需要打包的环境，dev开发环境，beta测试环境，prod生产环境

  ```bash
  $ mvn clean package -Dmaven.test.skip=true -Pdev
  ```

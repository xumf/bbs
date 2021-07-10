---
title : "去掉文中的^H字符"
date : "2018-05-09T23:36:22+08:00"
description : "About the Post"
tags : [
  "blog",
  "tutorial",
  "opini"
]
draft : false
---

> 注意这里的`^H`不是普通的`^H`，要通过按着`Control`，点`v`，然后点`h`，就会自动想形成，同理如果出现了`^M`的特殊符号，也要通过按着`Control`，点`v`，在点`m`，来去除，其它符号以此类推。

用vim打开文件，输入`:%s/^H//g`，回车

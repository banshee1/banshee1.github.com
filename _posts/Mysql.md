---
layout:     post
title:      "Mysql"
subtitle:   "tool"
date:       2021-1-1 11:50:00
author:     "Me"
header-img: "img/posts/titanic.jpg"
catalog: true
tags:
- Tool
---

[TOC]


# 连接

mysql -h 10.0.1.6 -u panda -p; pw@10086

# 查看表

```
--进入sql服务后首先查看有哪些数据库
show databases;

--若没有新建一个
CREATE DATABASE library;

--使用数据库
use library;
```

## commands
```
创建表
SHOW VARIABLES LIKE 'character_set_%';

设置主键
    alter table test add primary key (name);
    alter table test drop primary key, add primary key (name);


```
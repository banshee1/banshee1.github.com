---
layout:     post
title:      "Redis"
subtitle:   "tool"
date:       2021-1-1 11:50:00
author:     "Me"
header-img: "img/posts/titanic.jpg"
catalog: true
tags:
- Tool
---

[TOC]



# Commands

## 启动和连接

```
redis-cli -h host -p port -a password //远程

redis-cli //本地

redis-server //本地启动服务
```

# API

## key通用
```
### del
### exists
```

## string

### 简单API
```
### getset
```

### set

```
set key value
```

### get

```
set key value
 (若没有设置则返回 空 nil)
```



### 利用set一个string key实现锁

```
利用 set key value xx
```


```
del key
```

### mset

```
一次为多个key设置value
mset k1 v1 k2 v2 ...
```

### mget

```
mget key ...
返回值按照key顺序排序
```

### getrange

### setrange

### incrby

### decrby

```
incrby/decrby key 增量

** 
当遇到一个不存在的key, 会先将值初始化为0再 加减
```

### incrbyfloat

```
用途: 计数器
```


## Hash: 散列
### 简单API
```
### hget
### hstrlen
### hexists
### hdel
### hlen
```

### hset / hsetnx
	hset: 字段不存在时返回1, 存在时覆盖并返回0
	hsetnx: 只在字段不存在时设置返回1, 存在时不覆盖返回0

### hincrby
	hincrby hash field increment

### hlen
	获取hash包含的field=>value字段值
### hmset / hmget
### hkeys hvals hgetall






## List: 列表

## Set: 集合

## Sorted Set: 有序集合





# BitMap





# 命令

## 获取类型

```
TYPE KEY_NAME 
```



```
Redis的键值是无序放置在内存中的
```

### 覆盖规则

```
默认调用Set命令会覆盖原有的key, 但是可以使用 NX | XX 选项指定是否覆盖
	NX : 只有key不存在时才会赋值并且返回ok, 若key已存在则会返回nil表示设置失败
	XX : 只会对已经有value的key进行覆盖, 对于没有value的key和不存在的key会返回nil表示设置失败
```



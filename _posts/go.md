---
layout:     post
title:      "Go"
subtitle:   "Lang"
date:       2021-1-1 11:50:00
author:     "Me"
header-img: "img/posts/titanic.jpg"
catalog: true
tags:
- Lang
---


# 语法相关

## 自增(减) i++
```golang
    i++是语句不是表达式
所以:
    j = i++ 是非法的
```

## 变量声明
```go
s := ""
var s string
var s = ""
var s string = ""
```

## map
```
    make(map[string]int)

    go的字典在迭代时顺序是随机的, 而且是特意加的随机
```

# string 处理

## 拼接
```
strings.Join(array, " ")
```
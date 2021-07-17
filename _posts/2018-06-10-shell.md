---
layout:     post
title:      "shell"
subtitle:   "tool"
date:       2021-1-1 11:50:00
author:     "Me"
header-img: "img/posts/titanic.jpg"
catalog: true
tags:
- Tool
---

[TOC]

# shell samples

## cat | cut | sort
```bash
cat /etc/passwd | cut -d ':' -f 3 | sort -n -r | uniq

cut: 
    -d : 分割(后接分割符)
    -f : 分割后取出第几个字段(跟-d一起使用, 后跟数字)
    -c : 以字符为单位截取 (eg. -c 5-12)(取出5-12间的字符)

sort:
    -t : 分割(后接分割符)
    -k : 分割后以第几个区段排序
    -n : 以纯数字排序
    -r : 倒序

uniq:
    -i : 忽略大小写
    -c : 计数 (count)
```

## tee
```
双重定向

ls -l | tee -a log | grep test

-a : 累加
```

## grep
```
grep:
    -A after 加数字展示之后行数
    -B befer 之前



排除字符
    cat regular_express.txt | grep '[^g]oo'



```

## sed

```
sed '1,10d' 删除1-10行

sed '2i line2' 在第二行之前插入一行 'line2'
sed '2a line2' 之后

sed '2,5c No 2-5 Line' 替换

sed -n '1,10p' 显示1-10行

sed -i ** file 直接修改文件:
    sed -i '1,10d' test
```

## 正则表达式
```
^:
    []内是非, 括号外是行首符

$:
    行尾字符

*** 正则表达式中的 [] 是迭代的意思

```

## shell script
### 参数
```
shell脚本的参数:

sh name.sh  p1    p2    p3    p4
  ${0}     ${1}  ${2}  ${3}  ${4}

${i} 获取第几个参数
$# 参数的个数
$@ 拼接打印出所有参数

shift (n) 移除前面1(n)个参数
```
### if then
```
if [ cond ]; then
    content
elif [ cond2 ]; then
    ..
else
    ..
fi

[ cond1 -o con2 ]
[ con1 ] || [ con2 ]

```



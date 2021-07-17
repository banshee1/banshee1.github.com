---
layout:     post
title:      "PHP"
subtitle:   "Lang"
date:       2021-1-1 11:50:00
author:     "Me"
header-img: "img/posts/titanic.jpg"
catalog: true
tags:
- Lang
---


# PHP的$_Get

```php
<a href="text2.php?m=100&n=哦哈哈&w=ahaha&id='10'&name='xiaoqiang'">哦哈哈哈，点我啊</a>  
  
// text2.php
 echo $_GET["m"];                 //这里的输 出，以及下面的输出，都不能用 echo "$_GET["m"]";     (我试过了，加引号会显示错误，运行不出来)
 echo $_GET["n"];                  //因为我不太清楚这里get[ ]里面加不加引号的区别，以及传过来的值是数字或是字符串后的输出结果的区别，所以我都试了试
 echo $_GET[m];                  
 echo $_GET[n];
 echo "<br/>";
  
 echo $_GET["w"];
 echo $_GET[w];
 echo "<br/>";
 echo $_GET["id"];
 echo $_GET["name"];

 echo $_GET[id];

 echo $_GET[name];
```



# empty

```php
php > var_dump(empty(0));
bool(true)
php > var_dump(empty(""));
bool(true)
php > var_dump(empty(null));
bool(true)
php > var_dump(empty("0"));
bool(true)
php > var_dump(empty("1"));
bool(false)
php > var_dump(empty([]));
bool(true)
```




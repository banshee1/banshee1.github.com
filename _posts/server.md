---
layout:     post
title:      "Server Work Note"
subtitle:   "tool"
date:       2021-1-1 11:50:00
author:     "Me"
header-img: "img/posts/titanic.jpg"
catalog: true
tags:
- Notes
---

[TOC]



# 响应消息类型



## GeneralState



```
GeneralState
    // 通用状态消息，short
    table GeneralState {
      code:short = -1;
      msg:string;
      counter: [CounterDetail]; // 计数器当前值：转盘次数等
      state: [StateDetail]; // 当前状态：宝箱奖励、战令奖励等
      ext:string; // 扩展字符串，请求ID等
    }
    

GeneralList

```

## GeneralStateInt

```
    // 通用状态消息，int
    table GeneralStateInt {
      code:short = -1;
      msg:string;
      counter: [CounterDetail]; // 计数器当前值：转盘次数等
      state: [StateIntDetail]; // 当前状态：宝箱奖励、战令奖励等
      ext:string; // 扩展字符串，请求ID等
    }
```

## GeneralReward

```
GeneralReward
  // 通用奖励消息
  table GeneralReward {
    code:short = -1;
    msg:string;
    changes: [ItemsDetail]; // 道具增加部分，加奖励的内容
    balance: [ItemsDetail]; // 道具有变化部分的当前余额
    counter: [CounterDetail]; // 计数器当前值
    ext:string; // 扩展字段，IAP使用
  }
```





# 生成ssh rsa

```
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub
```



# 项目目录

 ```
├──code
		├──chat 聊天
		├──mail 右键
		├──team
		├──core
      ├── aowonline_apis   socket内部接口
      ├── aowonline_gate    下发nodejs ws支持
      ├── aowonline_nodejs  ws代码
      ├── aowonline_user
      ├── aowonline_web
      └── slggames_api   http接口
 ```



# php storm 设置断点

![image-20210513171920164](/Users/fanwenjie/Library/Application Support/typora-user-images/image-20210513171920164.png)

![image-20210513171931851](/Users/fanwenjie/Library/Application Support/typora-user-images/image-20210513171931851.png)



# 获取utc时间戳

```bash
date +%s
```



# 定时任务Crontab

```bash
crontab [-u username]　　　　//省略用户表表示操作当前用户的crontab
    -e      (编辑工作表)
    -l      (列出工作表里的命令)
    -r      (删除工作作)
    
我们用crontab -e进入当前用户的工作表编辑，是常见的vim界面。每行是一条命令。

crontab的命令构成为 时间+动作，其时间有分、时、日、月、周五种，操作符有

* 取值范围内的所有数字
/ 每过多少个数字
- 从X到Z
，散列数字
```

```
实例1：每1分钟执行一次myCommand
* * * * * myCommand
实例2：每小时的第3和第15分钟执行
3,15 * * * * myCommand
实例3：在上午8点到11点的第3和第15分钟执行
3,15 8-11 * * * myCommand
实例4：每隔两天的上午8点到11点的第3和第15分钟执行
3,15 8-11 */2  *  * myCommand
实例5：每周一上午8点到11点的第3和第15分钟执行
3,15 8-11 * * 1 myCommand
实例6：每晚的21:30重启smb
30 21 * * * /etc/init.d/smb restart
实例7：每月1、10、22日的4 : 45重启smb
45 4 1,10,22 * * /etc/init.d/smb restart
实例8：每周六、周日的1 : 10重启smb
10 1 * * 6,0 /etc/init.d/smb restart
实例9：每天18 : 00至23 : 00之间每隔30分钟重启smb
0,30 18-23 * * * /etc/init.d/smb restart
实例10：每星期六的晚上11 : 00 pm重启smb
0 23 * * 6 /etc/init.d/smb restart
实例11：每一小时重启smb
0 */1 * * * /etc/init.d/smb restart
实例12：晚上11点到早上7点之间，每隔一小时重启smb
0 23-7/1 * * * /etc/init.d/smb restart
```



# 软连接

软连接是linux中一个常用命令，它的功能是为某一个文件在另外一个位置建立一个同不的链接。

当 我们需要在不同的目录，用到相同的文件时，我们不需要在每一个需要的目录下都放一个必须相同的文件，我们只要在其它的 目录下用ln命令链接（link）就可以，不必重复的占用磁盘空间。

```
具体用法是：ln -s 源文件 目标文件。 
```

## 硬链接和软连接

```
【硬连接】
硬连接指通过索引节点来进行连接。在Linux的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号(Inode Index)。在Linux中，多个文件名指向同一索引节点是存在的。一般这种连接就是硬连接。硬连接的作用是允许一个文件拥有多个有效路径名，这样用户就可以建立硬连接到重要文件，以防止“误删”的功能。其原因如上所述，因为对应该目录的索引节点有一个以上的连接。只删除一个连接并不影响索引节点本身和其它的连接，只有当最后一个连接被删除后，文件的数据块及目录的连接才会被释放。也就是说，文件真正删除的条件是与之相关的所有硬连接文件均被删除。

【软连接】
另外一种连接称之为符号连接（Symbolic Link），也叫软连接。软链接文件有类似于Windows的快捷方式。它实际上是一个特殊的文件。在符号连接中，文件实际上是一个文本文件，其中包含的有另一文件的位置信息。
```


# Shell脚本
查找有15级以上英雄sh记录
```bash
#!env bash
for k in $(cat champion_invited_userid.txt);
do
  for j in $(redis-cli -h 10.0.1.6 hvals aow:hero:lv:$k);
  do
    if [ $j -gt 15 ]; then
        echo $k;
        break;
    fi
  done;
done;
```

# 关闭e_deprecated

```
修改:
  \error_reporting(E_ALL ^ E_DEPRECATED ^ E_NOTICE);
```


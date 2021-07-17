---
layout:     post
title:      "git"
subtitle:   "tool"
date:       2021-1-1 11:50:00
author:     "Me"
header-img: "img/posts/titanic.jpg"
catalog: true
tags:
- Tool
---

[TOC]



# 1. 从远端分支拉取新分支

```bash
git fetch origin feat-bp-task:feat-bp-task-wenjie
```

# 2. 撤销commit

```bash
git reset --soft HEAD^^
```

# 3. 回退到历史commit

```bash
git reset --hard 版本号
```

# 4. 查看upstream

```bash
git branch -vv
```



# 5. 删除远端分支

```bash
git push origin --delete [branch_name]
```



# 6. 设置upstream

```bash
git push --set-upstream origin (branch_name)
```

## 6-1. 设置upstream的其他方法

```bash
一般，本地分支如果是从clone或者fetch得到的，都在远程库有一个upstream分支。
设置upstream的方法有两种:
1.  push的时候(如上面)
2. 实际上，就是在修改 `.git/config`文件

[branch "my_local_branch_name"]
	remote = origin
	merge = refs/heads/my_remote_branch_name
```

# 7. 远端分支版本回退

https://www.cnblogs.com/Super-scarlett/p/8183348.html



# 8. 同步远端分支不产生Merge

```bash
git pull --rebase
```



# 9. 远端分支覆盖本地

```bash
git fetch --all
git reset --hard origin/master
```



# 10. Tig

```
https://blog.csdn.net/weixin_42465125/article/details/91346365
```

# 11. rebase
## git rebase -i
```
git rebase -i HEAD~10
向前rebase 10个commit

pick
r: 修改msg
s 合并到前一个pick或者r
```
## rebase main
```
git rebase --onto origin/main
	直接rebase到目标分支最后一个commit

git rebase -i commitId
	rebase到目标commit(不需要指定分支)
```






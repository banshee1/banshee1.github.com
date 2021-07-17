---
layout:     post
title:      "Docker"
subtitle:   "tool"
date:       2021-1-1 11:50:00
author:     "Me"
header-img: "img/posts/titanic.jpg"
catalog: true
tags:
- Tool
---

# 容器创建

 ## docker run

```bash
docker run ubuntu:15.10 /bin/echo "Hello World"
```

* docker : Docker的二进制执行文件
* run : 运行一个容器
* ubuntu*** : 指定要运行的镜像
* 在启动的容器里执行的命令

## 交互式容器

```bash
runoob@runoob:~$ docker run -i -t ubuntu:15.10 /bin/bash
root@0123ce188bd8:/#
root@0123ce188bd8:/# exit
exit
```

* -t : 在新容器内指定一个伪终端或终端(terminal)
* -i : 允许你对容器内的标准输入(STDIN)进行交互

## 后台创建容器

```bash
runoob@runoob:~$ docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
2b1b7a428627c51ab8810d541d759f072b4fc75487eed05812646b8534a2fe63
```

* 输出的长string是容器的唯一Id

  **CONTAINER ID:** 容器 ID。

  **IMAGE:** 使用的镜像。

  **COMMAND:** 启动容器时运行的命令。

  **CREATED:** 容器的创建时间。

  **STATUS:** 容器状态。

  状态有7种：

  * created（已创建）
  * restarting（重启中）
  * running 或 Up（运行中）
  * removing（迁移中）
  * paused（暂停）
  * exited（停止）
  * dead（死亡）

## 启动已停止的容器

```bash
$ docker ps -a
$ docker start b750bbbcfd88 
```



# docker 命令

## 启动容器

```bash
docker run -itd --name ubuntu-test ubuntu /bin/bash
```

-d : 后台运行容器, 并返回容器Id

--name : 指定容器名字

## 停止容器

```bash
docker stop <容器 ID>
```

## 重启容器

```bash
docker restart <容器 ID>
```

## 进入容器e

### attach

```bash
docker attach <Docker Id> 
```

**注意：** 如果从这个容器退出，会导致容器的停止。

### exec

```bash
docker exec -it 243c32535da7 /bin/bash
```

**注意：** 如果从这个容器退出，容器不会停止，这就是为什么推荐使用 **docker exec** 的原因。

更多参数说明请使用 **docker exec --help** 命令查看。

## 查看docker端口映射

```bash
docker port
docker port bf08b7f2cd89
```

## 查看日志

```bash
docker logs -f dockerId
```

**-f:** 让 **docker logs** 像使用 **tail -f** 一样来输出容器内部的标准输出。

从上面，我们可以看到应用程序使用的是 5000 端口并且能够查看到应用程序的访问日志。

## 查看docker内进程

```bash
runoob@runoob:~$ docker top wizardly_chandrasekhar
UID     PID         PPID          ...       TIME                CMD
root    23245       23228         ...       00:00:00            python app.py
```

## 移除容器

```bash
docker rm wizardly_chandrasekhar	
```

## 查看本地镜像(版本等)

```bash
docker images
```

同一仓库源可以有多个 TAG，代表这个仓库源的不同个版本，如 ubuntu 仓库源里，有 15.10、14.04 等多个不同的版本，我们使用 REPOSITORY:TAG 来定义不同的镜像。

所以，我们如果要使用版本为15.10的ubuntu系统镜像来运行容器时，命令如下：

## 指定镜像仓库版本

```bash
runoob@runoob:~$ docker run -t -i ubuntu:15.10 /bin/bash 
root@d77ccb2e5cca:/#
```

## 删除镜像

```bash
$ docker rmi hello-world
```

## 创建、更新镜像

### 更新镜像

从已经创建的容器中更新镜像，并且提交这个镜像

更新镜像之前，我们需要使用镜像来创建一个容器。

```bash
docker run -i -t ubuntu:15.10 /bin/bash
```

在运行的容器内使用 **apt-get update** 命令进行更新。

在完成操作之后，输入 exit 命令来退出这个容器。

此时 ID 为 e218edb10161 的容器，是按我们的需求更改的容器。我们可以通过命令 docker commit 来提交容器副本。

```bash
runoob@runoob:~$ docker commit -m="has update" -a="runoob" e218edb10161 runoob/ubuntu:v2
sha256:70bf1840fd7c0d2d8ef0a42a817eb29f854c1af8f7c59fc03ac7bdee9545aff8
```

使用我们的新镜像 **runoob/ubuntu** 来启动一个容器

```bash
runoob@runoob:~$ docker run -t -i runoob/ubuntu:v2 /bin/bash                            
root@1a9fbdeb5da3:/#
```

### 构建镜像

我们使用命令 **docker build** ， 从零开始来创建一个新的镜像。为此，我们需要创建一个 Dockerfile 文件，其中包含一组指令来告诉 Docker 如何构建我们的镜像。

```bash
runoob@runoob:~$ cat Dockerfile 
FROM    centos:6.7
MAINTAINER      Fisher "fisher@sudops.com"

RUN     /bin/echo 'root:123456' |chpasswd
RUN     useradd runoob
RUN     /bin/echo 'runoob:123456' |chpasswd
RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
EXPOSE  22
EXPOSE  80
CMD     /usr/sbin/sshd -D
```

每一个指令都会在镜像上创建一个新的层，每一个指令的前缀都必须是大写的。

第一条FROM，指定使用哪个镜像源。

RUN 指令告诉docker 在镜像内执行命令，安装了什么。

然后，我们使用 Dockerfile 文件，通过 **docker build** 命令来构建一个镜像。

```bash
runoob@runoob:~$ docker build -t runoob/centos:6.7 .
Sending build context to Docker daemon 17.92 kB
Step 1 : FROM centos:6.7
 ---&gt; d95b5ca17cc3
Step 2 : MAINTAINER Fisher "fisher@sudops.com"
 ---&gt; Using cache
 ---&gt; 0c92299c6f03
Step 3 : RUN /bin/echo 'root:123456' |chpasswd
 ---&gt; Using cache
 ---&gt; 0397ce2fbd0a
Step 4 : RUN useradd runoob
......
```

参数说明：

- **-t** ：指定要创建的目标镜像名
- **.** ：Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径



## 其他一些命令

```bash
docker ps -l 查询最后一次创建的容器
```



# Dockerfile

以定制一个ngix镜像为例

```bash
FROM nginx
RUN echo '这是一个本地构建的nginx镜像' > /usr/share/nginx/html/index.html
```

<img src="https://www.runoob.com/wp-content/uploads/2019/11/dockerfile1.png" alt="img" style="zoom:80%;" />

## 注意

Dockerfile 的指令每执行一次都会在 docker 上新建一层。所以过多无意义的层，会造成镜像膨胀过大。例如：

```bash
FROM centos
RUN yum install wget
RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz"
RUN tar -xvf redis.tar.gz
以上执行会创建 3 层镜像。可简化为以下格式：
FROM centos
RUN yum install wget \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
    && tar -xvf redis.tar.gz
```

如上，以 **&&** 符号连接命令，这样执行后，只会创建 1 层镜像。

## Build Image

```bash
docker build -t nginx:v3 .
```

在 Dockerfile 文件的存放目录下，执行构建动作。

以下示例，通过目录下的 Dockerfile 构建一个 nginx:v3（镜像名称:镜像标签）。

**注**：最后的 **.** 代表本次执行的上下文路径，下一节会介绍。

## 上下文路径

```bash
docker build -t nginx:v3 .
```



上下文路径，是指 docker 在构建镜像，有时候想要使用到本机的文件（比如复制），docker build 命令得知这个路径后，会将路径下的所有内容打包。

**解析**：由于 docker 的运行模式是 C/S。我们本机是 C，docker 引擎是 S。实际的构建过程是在 docker 引擎下完成的，所以这个时候无法用到我们本机的文件。这就需要把我们本机的指定目录下的文件一起打包提供给 docker 引擎使用。

如果未说明最后一个参数，那么默认上下文路径就是 Dockerfile 所在的位置。

**注意**：上下文路径下不要放无用的文件，因为会一起打包发送给 docker 引擎，如果文件过多会造成过程缓慢。





# Docker Compose

Compose 使用的三个步骤：

- 使用 Dockerfile 定义应用程序的环境。
- 使用 docker-compose.yml 定义构成应用程序的服务，这样它们可以在隔离环境中一起运行。
- 最后，执行 docker-compose up 命令来启动并运行整个应用程序



## dcp启动服务

```
docker-compose up --no-start --build
docker-compose up -d
```




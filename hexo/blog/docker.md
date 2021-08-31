---
title: Docker 基础学习
copyright: true
cover: https://www.notion.so/image/https%3A%2F%2Fwww.notion.so%2Fimages%2Fpage-cover%2Fmet_vincent_van_gogh_oleanders.jpg?table=block&id=1ccb6cd8-4b7c-4923-b8ae-f124619b7188&spaceId=a91d29d4-0c84-4d09-a043-0f0a2eb304f1&width=2880&userId=67312f76-c2f0-4efd-b763-84a0ffe82b96&cache=v2
top: 3
date: 2021-08-01 10:42:08
tags:
---

## 容器技术

应用场景：
- Web 应用的自动化打包和发布
- 持续集成与发布 CI、CD
- 自动化测试
<!-- more -->
### 容器

容器是镜像运行时的实体， 容器可以被创建、启动、停止、删除等
容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的 命名空间。因此容器可以拥有自己的 root 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。容器内的进程是运行在一个隔离的环境里，使用起来，就好像是在一个独立于宿主的系统下操作一样。这种特性使得容器封装的应用比直接在宿主运行更加安全。也因为这种隔离的特性，很多人初学 Docker 时常常会混淆容器和虚拟机
### 镜像

### 仓库
Docker 官方提供 [Docker hub](https://hub.docker.com) 公开的仓库

### 命令

查看 docker 相关信息
```bash
docker version
```

创建容器 
```bash
# 此时 container 可缩略、建议不缩略
$ docker [container] run nginx
```
查询容器相关信息
```bash
# 当前正在运行的容器
$ docker container ls
 或
$ docker container ps 旧版命令
```
参数： 
- -a :显示所有的容器，包括未运行的。
- -f :根据条件过滤显示的内容。
- --format :指定返回值的模板文件。
- -l :显示最近创建的容器。
- -n :列出最近创建的n个容器。
- --no-trunc :不截断输出。
- -q :静默模式，只显示容器编号。
- -s :显示总的文件大小

```
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                      PORTS     NAMES
69e1697ae1c2   nginx     "/docker-entrypoint.…"   20 minutes ago   UP 29 seconds ago             great_edison
5ab21df5360f   nginx     "/docker-entrypoint.…"   21 minutes ago   Exited (0) 20 minutes ago             exciting_bell
```
容器状态
 - created（已创建）
 - restarting（重启中）
 - running（运行中）
 - removing（迁移中）
 - paused（暂停）
 - exited（停止）
 - dead（死亡）

删除容器
```bash
$ docker container rm < container-name or container-id >
```
当删除正在运行的容器时，操作失败且会有 waring 提示

强制删除命令 后加 -f 选项
```bash
$ docker container rm < container-name or container-id > -f
```

组合命令 && 进行多容器操作

```bash
# 停止所有正在运行的容器
$ docker container stop $(docker container ps -q)
# 先查询所有正在运行的容器 ID 然后再执行 stop
```

Docker 端口映射
在开启端口映射之前，你首先要之道Docker对应的容器端口是多少。比如Nginx镜像的 80 端口。知道这个端口后，就可以在启动容器的时候，用-p <port:port> 的形式，启用映射了。

用Nginx举例:

第一个端口是映射到服务器本机的端口;第二个端口是Docker容器使用的端口。 比如你想把Docker的80端口，映射到服务器的9898端口。
```bash
$ docker container run -p 9898:80 nginx
```
等待项目启动后，打开浏览器窗口，在地址栏输入127.0.0.1:9898，就可以打开nginx的默认网址。
Docker端口映射成功

attached 前台运行模式

detached 后台运行模式

detached模式的开启方法，就是加一个参数-d或者--detach。

```Bash
$ docker run -d -p 80:80 nginx
```

detached 模式转 attached 模式

```bash
$ docker  attach <ID or Image Name>
```
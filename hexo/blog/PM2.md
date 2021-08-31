---
title: PM2
cover: https://cdn.pixabay.com/photo/2021/02/24/20/53/abstract-6047465__480.jpg
copyright: true
top: 2
date: 2020-07-27 17:51:06
updated: 2021-08-30 10:42
categories:
  - 笔记
---

PM2 是线上环境下 node 进程管理工具，可以利用它来简化很多 node 应用管理的繁琐任务，如性能监控、自动重启、负载均衡等。

## 主要特性

- 进程守护，应用崩溃自动重启
  > 开发中出现问题导致服务挂了，需要解决问题后重新 node index.js 手动重启服务，PM2 遇到错误能够自动重启，保证其他服务能够访问

- 启动多进程，自动做负载均衡，充分利用 CPU 和 内存
<!-- more -->
  - 为何使用多进程

    服务端： CPU 性能、内存、稳定性、安全性

    操作系统会限制一个进程的最大可用内存、单个进程的内存毕竟是受限的。单进程如果出错会导致整个服务崩溃，其他服务也无法正常提供；多进程如果一个进程崩溃了和其他进程没有太大的关系，能够提高服务器的稳定性

    能够充分利用多核 CPU 的优势

  - 多进程和 redis

    系统内存里面，多个进程之间资源是无法共享的，如 session，可以多个进程访问一个 redis，轻松实现数据共享
- 自带日志功能，记录自定义日志内容
  > PM2 会自动搜集项目中的 console.log、console.error 的输出日志到文件当中，应用日志会被保存到服务器硬盘目录~/.pm2/logs/。也可以自定义日志的输出路径
- 后台运行
  > 不像平时本地开发时，启动本地服务需要占用一个终端窗口，PM2 启动服务后在后台运行


### 安装

```bash
yarn global add pm2
```
默认会在安装用户目录下生成几个log文件  
/Users/heiliu/.pm2/logs/index-error.log          
/Users/heiliu/.pm2/logs/index-out.log            
/Users/heiliu/.pm2/pids/index-0.pid 

### 常用命令

- pm2 start <AppName>/<id>
- pm2 restart <AppName>/<id>
- pm2 stop <AppName>/<id>
- pm2 delete <AppName>/<id>
- pm2 info <AppName> / <id> // 查看基本信息
- pm2 list

### 启动服务

```sh
# 可以指定文件名 也可以不指定
$ pm2 start [文件名]

# 例如
$ pm2 start index.js
```

在业务开发与测试过程中，我们经常会遇到文件更新后需要重启业务的情况。对于本地环境我们可以使用如 Webpack Dev Server 或 nodemon 等工具监听文件变化，然后在文件发生改变后重新运行服务器。而 PM2 同样提供了类似的功能帮助我们实现这一需求

```sh
$ pm2 start --watch
```

这样只要当前目录下有任意文件发生改变，PM2都会尝试重启进程。

在使用这一参数的时候，有几个需要注意的地方：

1. 请在程序所在的目录执行启动命令，否则将会监视的不是程序所在目录，而是你执行目录当前所在的目录。  
2. 开启 `--watch` 参数后，就算你手动停止进程（不删除），进程也会在文件发生改变后自动启动，解决该问题的方法是在停止进程的时候加入如下参数：
```sh
$ pm2 stop [id] --watch
```

### 停止进程
```sh
$ pm2 stop [id|name]

$ pm2 stop all
```
也可以通过 namespace 关键字一次性停止该命名空间下的所有的进程


### 进程列表

```bash
$ pm2 list
```

### 某个进程信息

```sh
$ pm2 show [id]
```

### 日志查看
我们可以通过打开日志文件查看日志外，还可以通过 pm2 logs 来查看实时日志，这点有对于线上问题排查；日志查看命令如下：

```bash
$ pm2 logs
```

### 监控

监控进程的 CPU、内存、其他指定指标的具体使用情况或者日志输出

```sh
$ pm2 monit
```

### 负载均衡
自动给你做负载均衡  
```bash
pm2 start server.js -i (number|max)

# 开启三个进程运行项目
pm2 start app.js -i 3
# 根据机器CPU核数，开启对应数目的进程运行项目
pm2 start app.js -i max
```
### PM2的内置HTTP服务器
最后想介绍一个极为实用但极少有人提及的功能：  
HTTP 服务器、之前和很多同学探讨大前端项目前后端分离的时候，发现他们大多都使用 Nginx、Apache 甚至 Tomcat 来托管前端的静态页面，然后使用 PM2 来托管后端 API。但为了一个简单的前端页面专门撰写一堆配置文件，实在是太浪费时间了，PM2 可能也发现了这一点，于是贴心的内置了 HTTP 服务器：
```sh
$ pm2 serve [path] [port]
```

是的，就这么简单，一条命令就能启动一个 HTTP 服务器。由于这个 HTTP 服务器使用的是 NodeJS 实现，因此性能同样非常优异，在大部分情况下足够使用。如果是负载非常大的业务，一般也不会考虑使用 PM2，而会使用更具扩展性的Kubernetes。

### 配置

可以通过配置文件对 PM2（包括进程数量、日志文件目录等）进行配置

新建 pm2 的配置文件 pm2.config.json
修改 PM2 启动命令，使用配置文件重启
检查相应配置是否生效


```json
{
  "apps": {
    "name": "pm2-test",
    "script": "app.js",
    "watch": true, // 监听文件修改、自动重启进程
    "ignore_watch": ["node_modules", "logs"], // 忽略指定文件的修改 配合 watch
    "instances": 4, // 多进程、开启多进程日志文件也会存放在不同的文件中
    "error_file": "logs/error.log", // 自定义错误日志文件目录
    "out_file": "logs/out.log",
    "log_date_format": "YYYY_MM_DD HH:mm:ss" // 每条日志的日期输出格式
  }
}
```
修改启动命令
```json
$ pm2 start pm2.config.json
```

[Process Manager 2](https://pm2.keymetrics.io/docs/usage/quick-start/)

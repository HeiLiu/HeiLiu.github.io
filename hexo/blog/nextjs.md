---
title: 使用 Next.js 从零搭建项目
copyright: true
top: 100
date: 2020-11-05 14:02:49
tags: Next.js
---

npm init 初始化、生成 package.json 文件

安装 Next.js React 相关依赖

yarn add next react react-dom

配置 package.json 文件 scripts 脚本命令 

```json
"script": {
  "dev": "next -p 9393",
  "build": "next build",
  "start": "next start -p 9393",
}
```
说明: -p 指定端口号 默认 3000 端口; build 打包不需要指定端口

此时直接 start 会报错、根据 next 框架的设计 pages 是不可或缺的部分 也可以将 pages 放入 src 下面

---
title: Koa2
copyright: true
top: 100
date: 2019-12-13 11:50:34
tags:
---

 轻量级的 NodeJS server 框架

 安装
 使用脚手架安装

 ```bash
 $ yarn global add koa-generator
#  查看是否安装成功
$ koa2 -V
 ```

初始化
koa2 koa2-test

进入目录并安装依赖
```bash
$ cd koa2-test
$ yarn
```


Koa2 原生支持 async/await
// async/await 要点
// 1. await 后面可以追加 promise 对象，获取 resolve 的值
// 2. await 必须包裹在 async 里面，二者配套使用
// 3. async 函数执行返回也是一个 promise 对象
// 4. 可以使用 try-catch 捕获 promise 中 reject 的错误

新开发的框架和系统，都开始基于 Koa2，例如 egg.js

<!-- TODO -->
中间件机制

洋葱圈模型 🧅

[koa2](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651557535&idx=2&sn=6c51555c035718dad22f6d9ad48cb844&chksm=8025595eb752d048e39cebaeaa06cc1d95e2a56760483ed79de36a61c7301a184021aecaf1ba&scene=21#wechat_redirect)
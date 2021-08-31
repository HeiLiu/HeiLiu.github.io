---
title: browserslist
copyright: true
top: 100
date: 2020-07-27 13:52:25
tags:
---

在创建前端工程的时候，一个比较好的做法是制定你的工程上线之后主要支持的浏览器版本，在你支持的浏览器版本里面，你的项目运行没问题，不在范围的浏览器可能会出现一些高级 JS，css 特性不支持的 bug。哪些新的 ES6+的特性保留原样，哪些特性要转译成 es5，webpack，babel 本身是通过这个工具提供的浏览器支持范围来确定的

我们可以在 .babelrc 文件、package.json文件、browserslistrc中指定浏览器版本选项，优先级规则是 .babelrc文件定义了则会忽略 browserslistrc、.babelrc 没有定义则会搜索 browserslistrc 和 package.json 两者应该只定义一个，否则会报错。

[一文带你了解babel-preset-env](https://xiaozhuanlan.com/topic/3176840952)
---
title:  前端面试相关
copyright: true
top: 100
date: 2020-06-05 15:54:28
categories:
  - 面试
---

- [时间模型与事件循环](./EventLoop.md)


**!!**

const address = !!corpInfo || corpInfo.address;

如果要显式地将返回值（或者表达式）转换为布尔值，请使用双重非运算符（即!!）或者Boolean构造函数。

!! 相当于进行了两次取反, 使其他类型转换成 bool 类型。  
!! 两个叹号使 !!corpInfo 的值为 bool 类型，如果没有明确变量 corpInfo （非null/undifined/0/""等值），!!corpInfo 即为 true, 明确了变量的值的话  !!corpInfo 为 true。

```js
!null // true
!undefined // true
!0 // true
!'' // true

!!null // false
!!undefined // false
!!0 // false
!!'' // false
```
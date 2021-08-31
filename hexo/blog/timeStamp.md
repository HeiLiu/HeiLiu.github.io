---
title: 不同的时间戳获取方式对比
copyright: true
top: 100
date: 2020-08-06 22:22:30
tags:
---

在开发过程中经常会遇到时间戳的获取和对比  
不同的时间戳获取方式也存在性能的差异，虽然差异不会太大，但是也可以多考虑在日常开发中使用性能更佳的方式

经常用的时间戳的获取方式有：

```js
const timeStamp = new Date().getTime(); // number 类型

// new Date() 返回 实例化的时刻和日期 
const timeStamp = +new Date(); // 通过类型转化为时间戳 number

const timeStamp = Date.now(); // number
```

测试代码  
```js
 console.time('+new Date()')
  for(var i = 0; i < 650000; i++) {
    var o = + new Date()
  }
  console.timeEnd('+new Date()')

  console.time('new Date().getTime:')
  for(var j = 0; j < 650000; j++) {
    var p = new Date().getTime();
  }
  console.timeEnd('new Date().getTime')

  console.time('Date.now()')
  for(var k = 0; k < 650000; k++) {
    var q = Date.now()
  }
  console.timeEnd('Date.now()')

//  ------ 输出------
+new Date(): 146.077880859375ms
new Date().getTime: 99.68603515625ms
Date.now(): 63.155029296875ms
```

- Date.now()
Date.now() 用的时间最少，它与其它获取时间戳最大的区别就是，一个是 constructor 的 属性，其它是 constructor.prptotype 的属性，实例化的区别，显然实例化对象花的时间更多

- +new Date()
涉及到对象实例化、类型转换，转换成数字, 耗时最多

虽然差别不大，但是也算是一种性能的追求  

更多具体性能对比参考如下：  
[date.now() vs new Date()](https://jsperf.com/date-now-vs-new-date)
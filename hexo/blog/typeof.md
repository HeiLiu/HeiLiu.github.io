---
title: typeof 能够检测什么类型
date: 2021-08-30 22:16:58
categories:
  - 面试
---

typeof 适合基本数据类型和 Function 类型的检测, 无法检测 null 类型

[数据类型](type.md)

```js
  typeof null  // "object"
```

instance 适用于给自定义对象做类型检测

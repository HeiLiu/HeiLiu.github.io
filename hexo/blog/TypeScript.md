---
title: TypeScript 笔记
copyright: true
top: 100
date: 2020-07-21 15:42:09
tags:
---

## interface
常用于对 「对象形状」进行描述

```js
// 接口首字母一般大写 或者加上 I 前缀
interface Person {
  name: string;
  age?: number;
}

// 变量形状必须与接口形状一致，不多不少
let tom: Person = {
  name: "tom",
  age: 1
};

```

## 类型别名（type）

类型别名为类型创建新名称。  
类型别名有时与接口相似，但是可以命名基元，并集，元组和其他必须手工编写的其他类型。

```ts
type Name = string;
type StringOrNumber = string | number;
type ActionResponse = Promise<{ error?; data? }>;

type childItem = {
    id?: number;
    label: string;
    key?: string;
    info?: {
        price: string | number;
        specialPrices?: string | number;
    };
};

type menuItem = {
    label: string;
    key: string;
    children: Array<childItem>;
};
```


### interface 和 type 的区别 

- 语法区别
- interface 支持同名合并、type 不支持

[参考文档](https://juejin.im/post/5c2723635188252d1d34dc7d)

## 枚举
将可枚举的值定义为枚举类型 通常为一些常量  
枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射

- 普通枚举
```js
enum Days {
  Sun,
  Mon,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat
}

console.log(Days[5]); // Fri
console.log(Days["Sat"]); // 6

// 编译后结果
var Days;
(function (Days) {
    Days[Days["Sun"] = 0] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```
— 常量枚举

常量枚举在编译阶段会被删除，并且不能包含计算成员
```js
const enum Directions {
  UP, Down, Left, Right
}
```


### interface 和 class 的区别

类可以被实例化、可以实现接口  

接口可以被实现、扩展，但是不能被实例化

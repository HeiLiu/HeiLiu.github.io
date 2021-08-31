---
title: JS 基本数据类型和引用类型
date: 2018-07-30 15:01:13
tags: JavaScript
categories: 
  - 前端
copyright: true
---
 

## Js 基本数据类型  

js基本数据类型包括：`undefined`, `null`, `number`, `boolean`, `string`, `symbol`, `bigInt（新增）`。基本数据类型是按值访问的，就是说我们可以操作保存在变量中的实际的值  

### 1.基本数据类型的值是不可改变的  
 任何方法都无法改变一个基本类型的值是不可改变的，比如一个字符串：  

 ```js
  var name = "change";
  name.substr();//hang
  console.log(name);//change

  var s = "hello";
  s.toUpperCase()//HELLO;
  console.log(s)//hello  

 ```

 通过这两个例子， 我们原来发现定义的变量 name 的值始终没有发生改变，而调用 substr() 和 toUpperCase() 方法后返回的是一个新的字符串，跟原先定义的变量 name 并没有关系  


### 按值访问

 按值进行访问，操作的是保存在变量中实际的值 

### 不可添加方法属性

### 基础类型的比较是值的比较

### 基础类型存放在栈区 变量标识符 + 变量值

## 引用类型

### 同时保存在栈区和堆区中
栈区保存变量标识符和指向堆区的方法

## 基本包装类型（包装对象） 

---
title: RegExp 正则表达式
date: 2018-07-18 09:47:21
cover: https://cdn.pixabay.com/photo/2021/08/19/16/31/flowers-6558487__480.jpg
tags:
  - 前端
  - JavaScript
categories: 
  - 前端
  - Note
---

 
# RegRxp(正则表达式)  

> 正则表达式(regular expression)描述了一种字符串匹配的模式（pattern），可以用来检查一个串是否含有某种子串、将匹配的子串替换或者从某个串中取出符合某个条件的子串等,简化对字符串的操作  

<!-- more -->

## 什么是正则？  

规则、模式; 强大的字符串匹配的工具  

### 风格  
  js风格: const reg = new RegExp('a', i)  
  perl风格: const reg = /a/i  

### 转义字符  

  | 转义字符 | 说明 |  
  |-------- | ---|  
  | \d | 数字 [0-9] |  
  | \D | 除了数字 [^0-9]|  
  | \\ | 斜杆 \ |  
  | \n | 换行 |  
  | \w | 英文 数字 下划线 [a-z0-9_] |  
  | \W | 非英文 数字 下划线 [^a-z0-9_] |  
  | \s | 空格 |  
  | \S | 非空白字符 |  


### 特殊字符(元字符)  

  - [] 方括号   

    ```js  

      //  [abc] 表示a或者b或者c 
      const re = /[abc]pc/g
      // 范围匹配
      [a-z] [0-9] 
      [^a-z]  // 匹配非英文部分 

    ```

### 限定符(量词)  

- 基本形式： {n}表示出现的次数  

    ```
      // 8位电话号码  
      /[1-9]\d{7}/        // 以非零数字开头后面八位随意
    ```
- 表示出现次数范围： {n, m}  表示出现最少n次 最多m次  

```js
  /[1-9]\d{4, 10}/  // QQ 正则
```

- 不限次数：{n, } 表示最少n次， 最多不限  

    . 任意字符  
    *:  => {0, } 任意次  
    +: 若干 => {1, } 最少一次 最多不限  
    ?: 最少零次 最多一次 => {0, 1}  
    ^: 行首  
    $: 行尾  

```js
  (0\d{2,3}-)?[1-9]\d{7}  // 固话区号  010-2473544
```

### 方法  

- search  返回匹配的位置
- match 把所有匹配的东西都提取出来  
- replace 字符串替换,替换所有匹配的字符串，返回替换以后的字符串  
- test   检验字符串是否符合正则 返回Boolean  
> 特性 只要字符串一部分符合要求就返回true  

```js
  // 校验邮箱
  const email = '你好啊heiliu@Gmail.com'
  const re = /\w+@[a-z0-9]+\.[a-z]+/
  re.test(email)  // 后半部分符合 返回 true  
  const re1 = /^\w+@[a-z0-9]+\.[a-z]+$/ // 限制首尾
```

### 应用例子  

- 查找字符串中的数字并返回  

```js
  //  查找字符串中的数字  
  const str = '123ask32lks,alf21lksa12e45l3'
  const reg = /\d+/g    // global
  console.log(str.match(reg))
```

- 敏感词过滤  

```js
  // replace 的用例
  const reg = /匹配|出来/;
  const str = '把所有匹配的东西都提取出来 '
  console.log(str.replace(reg, '***'))  // 把所有***的东西都提取***
```

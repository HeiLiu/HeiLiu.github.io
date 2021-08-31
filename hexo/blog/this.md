---
title: this
copyright: true
top: 100
date: 2018-07-24 22:35:40
tags: 'JavaScript'
---

### this-用于访问当前方法所属的对象
```js
const Obj = {
  name: 'jack',
  fn() {
    console.log(this == Obj)；
  }
}

Obj.fn(); // true
```

```js
function showThis() {
  console.log(this);
}

show(); // window  在此时 show 相当于被 window 对象调用 本身属于 window 是 js 一开始 this 设计的错误 作者可能当时没有考虑清楚

// 在严格模式下 函数直接调用时 this 指向 undefined
'use strict';
function showThis() {
  console.log(this);
} 

showThis(); // undefined

setTimeout(showThis, 100); // window 此时的 this 指向 window 因为 setTimeout 属于 window 对象
//  相当于 
window.setTimeout(showThis, 100);
```

每个新生成的函数内部都会新建一个 this、这个 this 在函数被调用的时候被绑定  
this 在运行时进行绑定  
this 提供了一种更为优雅的方式隐式传递一个对象的引用，让 API 设计更加简洁且易于复用  

应用场景：  
- 普通函数中的 this 指向全局
- 构造器里的 this 指向 new 返回的新对象
- 函数作为方法被调用时，this 指向该对象
- 箭头函数不会创建自己的 this，使用一个封闭上下文中的 this

改变 this 的指向：
- .apply()
- .bind()
- .call()

forEach 中的 this 指向

```js
const myForEach(cb, thisArg) {
  for(let i = 0; i < this.length; i ++) {
    cb.call(thisArg, (this[i]));
  }
}

//  使用 forEach
const arr = [1, 2, 3];
arr.forEach(function(item){
  console.log(this, item); // undefined 1 undefined 2 undefined 3 此时的 this 指向为 undefined
})
```
  改变 this 指向 call 里面的参数
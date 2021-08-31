---
title: classnames 的用法总结
tags: 
categories: NPM
date: 2019-10-13
---

## React 和 Vue class对比 



在React项目开发中，类名的管理不像 Vue 那么方便;

如何为组件添加 CSS 的 class？
传递一个字符串作为 className 属性：

render() {
  return <span className="menu navigation-menu">Menu</span>
}
CSS 的 class 依赖组件的 props 或 state 的情况很常见：

render() {
  let className = 'menu';
  if (this.props.isActive) {
    className += ' menu-active';
  }
  return <span className={className}>Menu</span>
}

在官方的提示下推荐开发者使用 [classnames](https://github.com/JedWatson/classnames) npm 包来管理自己的类名会方便很多


```js
(function () {
  'use strict';
  // =>  Object.hasOwnProperty   用于判断某个成员是否在对象内
  var hasOwn = {}.hasOwnProperty;
  function classNames () {
     // 存储 className 值
    var classes = [];

    // 循环实参， arguments就是实际调用函数传入的参数，类似数组
    for (var i = 0; i < arguments.length; i++) {
      // 获取实参value
      var arg = arguments[i];

      // 跳过false条件 => false, null, undefined, NaN, 空, ...
      if (!arg) continue;

      // 判断传入参数的类型
      var argType = typeof arg;

      // 如果参数的类型是 string 或者 number
      if (argType === 'string' || argType === 'number') {
        // 直接追加到classes数组后面
        classes.push(arg);

      // 如果参数是数组并且长度大于0
      } else if (Array.isArray(arg) && arg.length) {
        // 调用自身函数，利用apply可以将数组转成字符串
        var inner = classNames.apply(null, arg);

        // 现在是一个字符串，隐士判断布尔值
        if (inner) {
          // 追加到数组后面
          classes.push(inner);
        }
      // 如果传入的参数是对象
      } else if (argType === 'object') {
        // 对object进行遍历
        for (var key in arg) {
          // 判断key是否存在arg对象内并且key的值隐士转换为true
          if (hasOwn.call(arg, key) && arg[key]) {
            // 将值追加到classes数组后面
            classes.push(key);
          }
        }
      }
    }
    // 将数组连接成字符串以空格拼接  => a b c
    return classes.join(' ');
  }

  // 如果是node.js环境运行
  if (typeof module !== 'undefined' && module.exports) {
    classNames.default = classNames;
    module.exports = classNames;

  // 如果用的requirejs模块管理 AMD
  } else if (typeof define === 'function' && typeof define.amd === 'object' && define.amd) {
    define('classnames', [], function () {
    return classNames;
  });

  // 否则运行于浏览器环境
  } else {
    window.classNames = classNames;
  }
}());
```
<!-- TODO -->

[classnames github库](https://github.com/JedWatson/classnames)
---
title: babel7 配置
copyright: true
top: 100
date: 2020-05-29 15:47:40
tags:
---

## babel, JS 编译器

Babel 是一个工具链，主要用于将 ECMAScript 2015+ （又可称为ES6，ES7，ES8等）版本的代码转换为向后兼容的 JavaScript 语法，以便旧版本浏览器或其他环境中运行代码

从 babel7 开始，所有的官方插件和主要模块，都放在了 @babel 的命名空间下。从而可以避免在 npm 仓库中 babel 相关名称被抢注的问题。  
如：  
babel-cli babel 6  
@babel/cli babel 7  

## 插件  

想要通过 `babel` 编译出兼容的代码、需要配合插件对源码进行语法和 `API` 转换处理


**插件在 `presets` 前执行，多个插件从前往后顺序执行**，插件配置：
```json
{
  "plugins": [
    [
      "@babel/plugin-transform-runtime"
    ]
  ]
}
```

### @babel/plugin-transform-runtime
> @babel/plugin-transform-runtime 可以重复使用babel注入的帮助程序、避免代码的多次注入，节省代码体积

主要作用：  
- 自动转换generators/async  
- 使用 core-js 按需给内置类型打上 polyfill, 和下面 presets配置重的 useBuiltIns: 'usage' 作用一样
- 通过 helpers 选项自动移除嵌入的babel helper,并且用module引用来代替。否则每个文件中都会加入这些inline babel helper,造成代码冗余。默认为ture。  

`@babel/plugin-transform-runtime` 需要依赖 `@babel/runtime`，二者配合使用  

`@babel/runtime` 作为运行时需要作为生产依赖进行安装，而`@babel/plugin-transform-runtime`作为开发依赖使用


对如下代码
```js
class Person {

}

// 不使用用 @babel/plugin-transform-runtime 经过`babel` 编译后的文件内容为  
"use strict";

function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

var Person = function Person() {
  _classCallCheck(this, Person);
};
```
可以看到在编译后的文件内 生成了 _classCallCheck() 模块、在项目中经常会在多个地方使用到 class 去创建类，那么在每一个文件内顶部都会生成 _classCallCheck() 模块，会极大的增加代码的体积。通过 @babel/plugin-transform-runtime 编译生成的代码文件如下：
```js
"use strict";

var _interopRequireDefault = require("@babel/runtime/helpers/interopRequireDefault");

var _classCallCheck2 = _interopRequireDefault(require("@babel/runtime/helpers/classCallCheck"));

var Person = function Person() {
  (0, _classCallCheck2.default)(this, Person);
};
```
在文件顶部没有直接注入之前的整个 function 模块、而是在 helpers 目录下生成模块，在编译转换需要的地方通过 require 引入。

## presets

`preset` 是 `babel` 提供的插件组合（一组插件）

`presets` 配置 使用数组，第一个表示 `preset` 名字，第二个表示配置参数， 如下：    
```json
{
  "presets": [
      [
          "@babel/preset-env",
          {
              "targets": {  // 配置目标环境，优化编译生成代码体积
                  "browsers": [
                      "> 1%",
                      "last 2 versions"
                  ]
              },
              "useBuiltIns": "usage", // 按需加载
              "corejs": 3
          }
      ],
      "@babel/preset-react"
  ]
}

```

**注意：`presets` 的执行顺序是由后往前的** 如上先执行 `@babel/preset-react` 再执行 `@babel/preset-env`  

### preset-env

将高级语法转换成低版本的写法  

`@babel/preset-env` 会根据你配置的目标环境，生成插件列表来编译。对于基于浏览器或 `Electron` 的项目，官方推荐使用 .browserslistrc 文件来指定目标环境
一般通过 package.json 中 browserlist 字段配置如：  
```json
  "browserslist": [
    "> 0.5%", // 市场份额
    "last 2 versions"  // 最近两个版本
  ],
```
~~那这个配置与babelrc中的target配置有啥联系吗？？~~  
尽量指定目标环境，保持编译代码体积最小

## 主要模块

### @babel/core babel 核心
包含 `babel` 的核心功能

### @babel/polyfill

> **V7.4.0** 版本开始，`@babel/polyfill` 被**废弃**，不建议使用 `polyfill` 了，需单独安装 `core-js` 和 `regenerator-runtime` 模块  

babel 几乎可以编译所有最新的 Javascript 语法，但对于 API 来说确并非如此

里面包含 core-js

### [core-js](https://github.com/zloirock/core-js)

- `core-js` 是 `JavaScript` 标准库的 `polyfill`
- core-js和 babel 高度集成，可以对 core-js的引入进行最大程度的优化


## 参考文档 && 链接




#### [关于 browsersList 配置](./browserslist.md)

[core-js@3 博客园](https://www.cnblogs.com/sefaultment/p/11631314.html)

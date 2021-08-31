---
title: 记一个伪元素的骚操作
date: 2019-10-25
tags: Skill
---

# 用伪元素实现气泡框

## 介绍 
  &nbsp;&nbsp;&nbsp;&nbsp;在项目开发中本来自己负责的基础组件库里面的 `Tooltip` 组件没有达到预期的效果(有八阿哥)...  
  &nbsp;&nbsp;&nbsp;&nbsp;后来想到其实可以用伪元素实现一个类似的气泡弹窗，但是又想到一个问题：我每个元素的气泡内容不一样这尼玛怎么填进去呢？  
  &nbsp;&nbsp;&nbsp;&nbsp;于是乎，查文档、还真在文档里让我发现了一点有用的东西，通过 `attr()` CSS表达式和一个`自定义数据属性` data-descr 创建一个纯CSS, 内容提示气泡如下：  

<iframe
     src="https://codesandbox.io/embed/tooltip-tr1eg?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="tooltip"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

相关属性： 自定义属性 data-desrc 表达式 attr()  

兼容性： 还不错哦！！

## 各种链接如下：  
- [ data-descr_Demo链接](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::after)

- [attr() 具体文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/attr)

- [canIUse](https://caniuse.com/#search=attr()) 
- [Demo地址](https://codesandbox.io/s/crazy-chatterjee-9h066?fontsize=14)

## 相关代码

```html
<h1>伪元素实现气泡提示</h1>
<p>
  <span class="tooltip" data-descr="伪元素">伪元素</span>
  <span class="tooltip" data-descr="实现">实现</span>
  <span class="tooltip" data-descr="气泡提示">气泡提示</span>
</p>
```

```css
body {
  font-family: sans-serif;
}

.tooltip {
  position: relative;
  display: inline-block;
  cursor: pointer;
}

.tooltip[data-descr]:hover::after {
  content: attr(data-descr);
  position: absolute;
  top: -24px;
  left: 0;
  min-width: 60px;
  max-width: 100%;
  height: 24px;
  padding: 0 8px;
  box-sizing: border-box;
  line-height: 24px;
  font-size: 10px;
  text-align: left;
  color: #fff;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  border-radius: 4px;
  background: #202020;
}

.tooltip[data-descr]:hover::before {
  content: "";
  position: absolute;
  top: 0px;
  left: 15px;
  padding: 1px;
  box-sizing: border-box;
  border: 5px solid #202020;
  width: 0;
  height: 0;
  border-left-color: transparent;
  border-right-color: transparent;
  border-bottom-color: transparent;
  z-index: 99;
}
```
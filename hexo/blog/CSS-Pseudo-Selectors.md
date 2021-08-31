---
title: CSS 伪选择器(CSS_Pseudo-Selectors)
copyright: true
top: 100
date: 2020-05-22 12:21:48
tags:
---

> 选择器权重： 0级只没有优先级，1级是标签选择器，10级是累选择器、属性选择器，100级是 ID 选择器
## 伪类
<!-- TODO 翻一下原来的笔记补充一下 -->
伪类优先级和类选择器一致，都是10

逻辑伪类 | 作用
:not() | 取反（反选伪类）
:is() | 判断
:where() | 条件

但是对于逻辑伪类来说，优先级为0，例如： :not(), :is(), :where() 等

逻辑伪类的优先级由括号里面的选择器决定，支持多个选择器通过逗号隔开，不支持及联选择器

```css
:not(.disabled) {}    /* 优先级等同于.disabled选择器 */
:not(a) {}    /* 优先级等同于a选择器 */
/* 既不是 div 也不是 span 的元素 */
:not(div, span) {} 
:not(a.disabled) {}    /* 无效，不支持 */
/* 需要拆开写 */
:not(a):not(.disabled) {}
```
状态伪类 | 作用 
--- | ---
:link | 未访问过的超链接
:visited | 访问过的超链接
:hover | 鼠标悬停的元素
:actived | 选取点中的元素
:focus | 获得焦点的元素

筛选伪类 | 作用
--- | ---
:first-child | 当前选择器下第一个元素
:last-child | 当前选择器下最后一个元素
:nth-child(n) | 选取第n个，:nth-child(2n + 1) 奇数位
:checked | 只对 radio 和 checkbox 有效，选取当前选中的元素
:disabled | 选取禁用的表单元素
:only-child | 选取唯一子元素。如果一个元素的父元素只有它一个子元素，这个伪类就会生效。如果一个元素还有兄弟元素，这个伪类就不会对它生效。

## 伪元素
伪元素的权重和标签选择器权重相同，为 1
 常用的伪元素：  
::after，::before :，
其中 content 默认是行内元素

伪元素 | 作用  
--- | ---
::before|在某个元素之前插入一些内容
::after|在某个元素之后插入一些内容
::first-line | 为某个元素的第一行文字使用样式
::first-letter | 为某个元素中的文字的首字母或第一个字使用样式
::selection|对光标选中的元素添加样式。

[:after + data-descr 实现tooltip效果](./beforeAfter.md)
- 不能通过 js 去操作伪元素生成的内容，因为伪元素构造的元素是虚拟的。
- first-line 和 first-letter 不适用于内联元素，在内联元素中这两个选择器都会失效。
- 除了 selection，其余四个伪元素选择器已经在 CSS2 中存在且和伪类用的是一样的单冒号表示的。为了向下兼容，现在的浏览器中伪元素选择器用单冒号和双冒号都可以。

### 伪类与伪元素的区别

伪类的效果可以通过添加一个实际的类来达到，而伪元素的效果则需要通过添加一个实际的元素才能达到，这也是为什么他们一个称为伪类，一个称为伪元素的原因。  
伪元素和伪类之所以这么容易混淆，是因为他们的效果类似而且写法相仿，但实际上 css3 为了区分两者，已经明确规定了伪类用一个冒号来表示，而伪元素则用两个冒号来表示。

[sdfas](https://blog.bitsrc.io/css-pseudo-selectors-you-never-knew-existed-b5c0ddaa8116)
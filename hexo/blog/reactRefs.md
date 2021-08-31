---
title: React中的Refs
date: 2019-10-26 22:33
tags: React
categories:
  - 前端
  - React
---

## Refs

**官方说明**： `Refs 提供了一种方式，允许我们访问 DOM 节点或在 render 方法中创建的 React 元素`。

在React开发中、想要操作元素的状态一般是修改State、或者是修改传入的props。但是有时候一些效果不能通过如此操作实现，例如开发中常常碰到的： 

  - 输入框的焦点获取、比如打开登录界面登录框自动获取焦点 
  - 动态的根据一个元素的大小/距离 计算另外一个元素的大小 

## 使用 Ref

目前的React版本（16.11.0）中 `Refs` 的用法如下： 

```js
import React form 'react';
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    // 创建
    this.myRef = React.createRef();
  }

  render() {
    // 传入
    return <div ref={this.myRef} />;
  }
}
```
对于节点的访问: 
```js
const node = this.myRef.current;
```

说明： 
  - 1.通过调用 React.createRef 创建了一个 React ref 并将 DOM节点的引用current赋值给 this.myRef 变量。 
  - 2. 指定 ref 为 JSX 属性，将this.myRef 传入; ref 和 key 一样不属于 props属性，二者都会被 React 特殊处理和维护。
  - 3. ref 挂载以后，ref.current 指向 ref 所在节点

如果之前用过 React，你可能了解之前的 ref 可以通过 this.refs.inputRef 来访问 DOM 节点、如下所示，这个 ref 是字符串类型的，在使用上来说似乎更加方便。现在官方版本不建议再使用它，因为 string 类型的 refs 存在问题。它属于`过时` API 并可能会在未来的版本被移除。

```js
import React form 'react';
class MyInput extends React.Component {
  componentDidMount() {
    this.refs.inputRef.focus();
  }
  render() {
    // 还能用 但不建议
    return <input ref="inputRef" />;
  }
}
```

## Ref的值 

官方文档中有说明，ref 的值根据节点的类型不同而有所不同：

- 当 ref 属性用于 HTML 元素时，构造函数中使用 React.createRef() 创建的 ref **对象** 接收底层 DOM 元素作为其 current 属性。可以访问元素的宽高等属性、input框还可以调用focus方法实现自动聚焦
- 当 ref 属性用于自定义 class 组件时，ref 对象接收组件的挂载实例作为其 current 属性。即可以通过 current `调用组件中的方法 `

不能在函数组件上使用 ref 属性，因为他们没有实例。

## 回调 Refs 

更加精细的控制 refs 的传递, 可以达到类似 props 的传递效果，在需要的地方传入inputRef 

```js
// 官方 Demo
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```

**官方说明**： 

如果 ref 回调函数是以内联函数的方式定义的，在更新过程中它会被执行两次，第一次传入参数 null，然后第二次会传入参数 DOM 元素。这是因为在每次渲染时会创建一个新的函数实例，所以 React 清空旧的 ref 并且设置新的。通过将 ref 的回调函数定义成 class 的绑定函数的方式可以避免上述问题，但是大多数情况下它是无关紧要的。

## Refs 转发


## ref Hook

useRef() 是 React 提供的在 Hooks 中获取 DOM 元素的方法。
使用方法如下：
```js
import React, { useRef} from 'react';
function RefDemo(){
    const inputEl = useRef(null);
    const onButtonClick=()=>{ 
        inputEl.current.value="Hello, Ref";
        console.log(inputEl); //输出获取到的DOM节点
    };
    return (
        <>
            {/*保存input的ref到inputEl */}
            <input ref={inputEl} type="text"/>
            <button onClick = {onButtonClick}>展示</button>
        </>
    );
}
export default RefDemo;
```


<!-- TODO -->

[官方文档](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html)
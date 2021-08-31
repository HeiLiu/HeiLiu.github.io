---
title: React Hook学习
tag: React
categories:
  - 前端
  - React
date: 
---

  Hooks 在React **16.8** 以上的版本中才可以使用  
  [eslint-plugin-react-hooks](https://www.npmjs.com/package/eslint-plugin-react-hooks)
## Hook 定义

> Hook 是一些可以让你在函数组件里“钩入” React state 及生命周期等特性的`函数`。  
<!-- more -->
   Hook 不能在 class 组件中使用 —— 它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。  
   使组件自身在初次 render 之后，能够通过 Hook 机制再触发状态的变更并且引起re-render。

```js
// Hook类型定义 链表
type Hook = {
  memoizedState: any, // 存储最新的state 链表
  baseState: any,
  baseUpdate: Update<any, any> | null,
  queue: UpdateQueue<any, any> | null, // 更新队列
  next: Hook | null, // 下一个hook
}
```

### React 并不会放弃 class 

#### Class 的缺陷： 

  - this 的指向问题，在函数组件的编写中经常会碰到this的指向问题 
  - 编译过后的代码大小 
  - Javascript实现的类本身比较鸡肋，没有类似Java/C++多继承的概念，类的逻辑复用是个问题 
  - Class Component在React内部是当做Javascript Function类来处理的

## Hook 使用规则 

  Hook 就是 JavaScript 函数，但是使用它们会有两个额外的规则：

  - 只能在函数最外层调用 Hook。**不要在循环、条件判断或者子函数中调用**。
  - `只能`在 React 的函数组件或者自定义的Hook中调用 Hook。

    在实际编写底层组件库中常常会配合 useState Hook 进行测试, 因为我们的最底层的组件通常应该是被设计成 `stateless` 的，需要外部传入props 进行控制测试

##  相关 Hook 的使用

### useState Hook  
  在函数组件中、通过 useState Hook 可以使用在 class 中的 state 特性；函数退出后、state 中的变量会被 React 保留；usetState 返回一个状态以及这个状态的 setter 方法，

**初始化**  
一  
```js
const [state, setState] = useState(initialState)
```
二  
在初始化的时候、如果state需要通过计算获得、或者需要进行比较 expensive 的计算，可以传入一个函数。  
这个 initialState 参数只会在组件的初始渲染中起作用，后续渲染时会被忽略
```js
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
});
```
```js
function Table(props) {
  // ⚠️ createRows() 每次渲染都会被调用
  const [rows, setRows] = useState(createRows(props.count));
  // ...
}

function Table(props) {
  // ✅ createRows() 只会被调用一次
  const [rows, setRows] = useState(() => createRows(props.count));
  // ...
}
```

**修改状态**

  - state 初始化之后只能通过这个 setter 修改对应这个 state。  
  
  - 框架内部会对多次 setter 操作进行合并（循环执行传入的setter，目的是保证 useState 拿到最新的状态）
  
  - state 比较多的情况下可以使用对象、数组的形式、在修改state的时候需要注意，**相较于 setState 非覆盖式更新状态，useState 覆盖式更新状态(会替换 state 的值)，需要开发者自己处理逻辑。**

```js 
// 通过新的状态更新
setState(updateState);
// 函数式更新
setState(preState => preState + 10);
```

  <iframe
     src="https://codesandbox.io/embed/heuristic-driscoll-hskqs?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="heuristic-driscoll-hskqs"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

### useEffect Hook 

```js
export type Effect = {|
  tag: HookEffectTag,
  create: () => (() => void) | void, // 存储 useEffect 传入的 callback
  destroy: (() => void) | void, // 存储 useEffect 传入的 callback 的返回函数，
  deps: Array<mixed> | null,
  next: Effect,
|};
```
Effect 处理函数组件中副作用：数据获取，设置订阅以及手动更改 React 组件中的 DOM

useEffect 必须在组件内部调用，可以直接在作用域内读取 state 或者 props  

与 useState 传入具体 state 不同，useEffect传入的是一个 callback 函数。  
与useState 最大的不同是执行时机，useEffect 是在**组件被渲染为真实DOM后执行**  

  > 默认情况下，在第一次渲染之后和每次更新之后都会调用 useEffect 中的函数,它让我们在函数组件中存储内部 state 

Function Component 不存在生命周期，不用再去考虑"挂载"还是"更新"，即原来在 class 组件中的 componentDidMount、componentDidUpdate 和 componentWillUnmount。 

与 componentDidMount 或 componentDidUpdate 不同，使用 useEffect 调的 effect 不会阻塞浏览器更新屏幕，这让你的应用看起来响应更快(Effect 是异步操作)。大多数情况下，Effect 不需要同步地执行。在个别情况下（例如测量布局）有单独的 useLayoutEffect Hook 供你使用，其 API 与 useEffect 相同。

  ```js
    import React, { useState, useEffect } from 'react';

    const Example = () => {
      const [count, setCount] = useState(0);

      // 在执行 DOM 更新之后调用
      useEffect(() => {
        // 在render后输出点击的次数
        console.log(`You clicked ${count} times`);
      });

      return (
        <div>
          <p>You clicked {count} times</p>
          <button onClick={() => setCount(count + 1)}>
            CLICK CRAZY!
          </button>
        </div>
      );
    }
  ```
 - 回收机制：useEffect 接受传入的 callback 返回一个函数来指定如何“清除” effect。  
    在组件被销毁时会执行返回的回调函数。

 - 如果传入第二个参数，监听某个state变化而执行、实现性能优化，在监听的元素发生变化后才调用 effect; 若传入空的依赖数组 []，意味着该 hook 只在组件挂载时运行一次

<iframe
     src="https://codesandbox.io/embed/useeffectdeyilai-3jmdl?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="useEffect的依赖"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>
  
  ```js
    import React, { useState, useEffect } from 'react';

    function Example() {
      const [count, setCount] = useState(0);

      // 可以添加第二个参数、只要第二个参数发生变化、return中的方法也会执行
      useEffect(() => {
        console.log(`You clicked ${count} times`);
        // 组件销毁时执行
        return () => {
          console.log('Bye...');
        }

      }, [count]);

      return (
        <div>
          <p>You clicked {count} times</p>
          <button onClick={() => setCount(count + 1)}>
            CLICK CRAZY!
          </button>
        </div>
      );
    }
  ```

### Hook 使用了 JavaScript 的闭包机制 

将 useEffect 或者其他 Hook 放在组件内部让我们可以在 effect 中直接访问 state 变量（或其他 props）

  **capture value 特性**  

  非 useRef 的 hook 本质上都形成了闭包，拥有自己独立的状态（因为闭包的值实际上不是 reactive 的，）。  

  可以认为每次 render 的内容都会形成一个快照并保留下来， 当状态变更时，就形成了多个 Render 状态，而每个 Render 状态都拥有自己固定不变的 Props 与 State，内部的函数在每次渲染时也是独立的。  

 - 所有 Hook 除了 useRef 都具有 capture value 特性  
 - 每次 Render 都有自己的 Effects
 - 每次 Render 都有自己的 state 和 props
  
  会导致有的时候拿到的 state 或者 props 是旧值。


### useReducer  

useState()的替代方案  
提供了在组件外重新编排 state 的能力
useReducer 返回的结构与 useState 很像，只是数组第二项是 dispatch，而接收的参数也有两个，初始值放在第二位，第一位就是 reducer。

**React 会确保 dispatch 函数的标识是稳定的，并且不会在组件重新渲染时改变**，useReducer返回的dispatch对象又是“性能安全的”，可以直接放心地传递给子组件而不会引起子组件re-render。所以说 **把 dispatch 放入依赖数组没什么意义**。

**指定初始化state**
```js
// 1 state = initialState
const [state, dispatch] = useReducer(reducer, initialState);
// 2 state = init(initialArg) init() 用于计算 state
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

<!-- 基于Hooks Api可以实现一个useReducer Hook -->
```js
const useReducer = (reducer, initialArg, init) => {
  const [state, setState] = useState(
    init ? () => init(initialArg) : initialArg,
  );
  const dispatch = useCallback(
    action => setState(prev => reducer(prev, action)),
    [reducer],
  );
  return useMemo(() => [state, dispatch], [state, dispatch]);
};
```

<iframe
     src="https://codesandbox.io/embed/small-cloud-tmriz?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="useReducer"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

```js
function reducer(state, action) {
  // 能够拿到组件的所有的 state
  switch (action.type) {
    case "INCREMENT":
      return {
        ...state,
        count: state.count + state.step
      };
    case "DECREMENT":
      return {
        ...state,
        count: state.count + state.step
      };
  }
}

function Counter(props) {
  const [state, dispatch] = useReducer(reducer, {
    count: 0,
    step: 10,
  });
  const { count, step } = state;

  useEffect(() => {
    const timer = setInterval(() => {
      dispatch({ type: "INCREMENT" });
    }, 1000);
return () => clearInterval(timer);
    // 依赖 dispath 没有多大的意义
  }, [dispatch]);

  return <h1>{count}</h1>;
}
```

### useContext

数据透传、减少组件层级、简化了消费 context 的过程

useContext 的参数必须是 context 对象本身

调用了 useContext 的组件总会在 context 值变化时重新渲染  
<iframe
     src="https://codesandbox.io/embed/friendly-morse-zzxrn?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="friendly-morse-zzxrn"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

### useCallback

```js
const memorizeFn = useCallback(fn, deps)
```
返回该回调函数 fn 的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新  

经过 useCallback 包装过的函数可以当作普通变量作为 useEffect 的依赖。useCallback 做的事情，就是在其依赖变化时，返回一个新的函数引用，触发 useEffect 的依赖变化，并激活其重新执行。

```js
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const getFetchUrl = () => {
      return "https://v?query=" + count;
    }

    getFetchUrl();
  }, [count]);

  return <h1>{count}</h1>;
}
// 利用 useCallback() 将函数放到 useEffect 外部
function Counter() {
  const [count, setCount] = useState(0);

  const getFetchUrl = useCallback(() => {
    return "https://v?query=" + count;
  }, [count]);

  useEffect(() => {
    getFetchUrl();
  }, [getFetchUrl]);

  return <h1>{count}</h1>;
}
```
<!-- 反而更容易使性能变差 -->

### useMemo

```js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

```js
// useCallback 不会执行 fn 而是将其返回， useMemo 则会执行 fn 返回具体的值
useCallback(fn, deps) => useMemo(() => fn, deps)
```

**执行时机：** 在渲染期间执行，useMemo() 的返回值参与 render  
  把“创建”函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时才重新计算 memoized 值。这种优化有助于避免在每次渲染时都进行高开销的计算，用于缓存一些耗时的计算结果，只有当依赖参数改变时才重新执行计算

  如果没有提供依赖项数组，useMemo 在每次渲染时都会计算新的值。
  若第二个参数为空数组，则只会在渲染组件时执行一次，传入的属性值的更新也不会有作用。

```js
function Parent({ a, b }) {
  // Only re-rendered if `a` changes:
  const child1 = useMemo(() => <Child1 a={a} />, [a]);
  // Only re-rendered if `b` changes:
  const child2 = useMemo(() => <Child2 b={b} />, [b]);
  return (
    <>
      {child1}
      {child2}
    </>
  )
}
```

### useRef 

> useRef 会在每次渲染时返回同一个 ref 对象,  可以认为 ref 在所有 Render 过程中保持着唯一引用。也可以认为，ref 是 Mutable 的，而 state 是 Immutable 的

- 用 useRef 获取 React JSX 中的 DOM 元素  
- 用useRef来保存变量  
- useRef是所有Hooks API里边唯一一个返回mutable数据的
- 修改useRef值的唯一方法是修改其current的值，且值的变更不会引起re-render
- 每一次组件render时useRef都返回固定不变的值，

### 自定义 hook

> 创建规则： 函数名遵循以 use 开头，且返回非 JSX 元素

- 使用自定义 Hook 从组件中提取状态逻辑，使得这些逻辑可以单独测试并复用。

<iframe
     src="https://codesandbox.io/embed/interesting-sid-3tvcc?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="customHook-usePrevious"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>


### 参考
- [React Hooks 文档](https://zh-hans.reactjs.org/docs/hooks-overview.html) 

- [React Hooks 完全上手指南](https://zhuanlan.zhihu.com/p/92211533?utm_source=wechat_session&utm_medium=social&utm_oi=29558355001344)
- [函数组件与类组件](https://overreacted.io/zh-hans/how-are-function-components-different-from-classes/)
- [精读《useEffect 完全指南》](https://github.com/dt-fe/weekly/blob/v2/096.%E7%B2%BE%E8%AF%BB%E3%80%8AuseEffect%20%E5%AE%8C%E5%85%A8%E6%8C%87%E5%8D%97%E3%80%8B.md)
- [a-complete-guide-to-useeffect](https://overreacted.io/zh-hans/a-complete-guide-to-useeffect/)
- [React 作者关于 Hooks 的深度 issue](https://zhuanlan.zhihu.com/p/53375744)

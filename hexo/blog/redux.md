---
title: redux 笔记
copyright: true
top: 100
date: 2020-06-01 15:36:22
tags: redux
---

## Action

### 定义

Action 本质是 JS 对象，是把数据从应用传到 store 的唯一来源， 一般通过 store.dispatch(action) 将 action 传到 store。action 只是描述有事情发生
```js
const ADD_TODO = 'ADD_TODO';

{
  type: ADD_TODO,
  text: 'Build my first Redux app'
}
```

- action 内必须使用一个字符串类型的 type 字段来表示将要执行的动作（通常为常量）  
- 当 action 过多的时候，使用单独的文件进行管理 actionTypes

### action 创建函数  

Action 创建函数 就是生成 action 的方法。“action” 和 “action 创建函数” 这两个概念很容易混在一起，使用时最好注意区分。

```js
const ADD_TODO = 'ADD_TODO';

function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```
这样做将使 action 创建函数更容易被移植和测试  

Redux 中只需把 action 创建函数的结果传给 dispatch() 方法即可发起一次 dispatch 过程。

```js
dispatch(addTodo(text));
```
store 里能直接通过 store.dispatch() 调用 dispatch() 方法，但是多数情况下你会使用 react-redux 提供的 connect() 帮助器来调用。  
使用 connect() 前，需要先定义 mapStateToProps 这个函数来 **指定如何把当前 Redux store state 映射到展示组件的 props 中**。

```js
class DemoComponent extends React.Component {
  // ...
}

const mapStateToProps = state => ({
    text: state.text,
});

export default connect(mapStateToProps)(DemoComponent);
```

除了 state，还可以分发 dispatch 
```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'

const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
  }
}

const mapStateToProps = state => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}

const mapDispatchToProps = dispatch => {
  return {
    onTodoClick: id => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

## 异步 Action
<!-- TODO -->

## reducer
Reducers 指定了应用状态的变化如何响应 actions 并发送到 store 的，描述如何更新 state。

Reducer 是纯函数，它不应做有副作用的操作，如 API 调用或路由跳转。这些应该在 dispatch action 前发生

```js
import {
  ADD_TODO,
  TOGGLE_TODO,
  SET_VISIBILITY_FILTER,
  VisibilityFilters
} from './actionTypes'

function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: action.text,
            completed: false
          }
        ]
      })
    default:
      return state
  }
}
```
```js
Object.assign(state, {visibilityFilter: action.filter}) // ❌
Object.assign({}, state, {visibilityFilter: action.filter}) 
```
- 不要修改 state， object()将第一个参数设为空对象，第一种会修改 state 的值  
- object.assign() 是 ES6 新特性，存在兼容性问题，要么使用 @babel/polyfill babel 插件，要么使用其他库 _assign()  
- 在 default 情况下返回旧的 state。遇到未知的 action 时，一定要返回旧的 state

reducer 拆分： 可以将数据和单纯 UI 控制相关进行拆分

通过 combineReducers() 将拆分的 reducers 进行 合并
```js
import { combineReducers } from 'redux'

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

export default todoApp
```

```js
export default function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
```
两种方法等价
## store
store 是将 action 和 reducer 二者联系起来的对象  
再次强调一下 Redux 应用只有一个单一的 store。当需要拆分数据处理逻辑时，你应该使用 reducer 组合 而不是创建多个 store


通过 redux 提供的 createStore() 方法根据已有的 reducer 来创建 store, 可以通过第二个参数提供 initState 初始化应用
> const store = createStore(reducers, [initState])

```js
import { createStore } from 'redux'
import todoApp from './reducers'
let store = createStore(todoApp)
```
store的职责：  
-维持应用的 state；
- 提供 getState() 方法获取 state
- 提供 dispatch(action) 方法更新 state
- 通过 subscribe(listener) 注册监听器
- 通过 subscribe(listener) 返回的函数注销监听器

```js
import {
  addTodo,
  toggleTodo,
  setVisibilityFilter,
  VisibilityFilters
} from './actions'

// 打印初始状态
console.log(store.getState())

// 每次 state 更新时，打印日志
// 注意 subscribe() 返回一个函数用来注销监听器
const unsubscribe = store.subscribe(() => console.log(store.getState()))

// 发起一系列 action
store.dispatch(addTodo('Learn about actions'))
store.dispatch(addTodo('Learn about reducers'))
store.dispatch(addTodo('Learn about store'))
store.dispatch(toggleTodo(0))
store.dispatch(toggleTodo(1))
store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))

// 停止监听 state 更新
unsubscribe()
```

## 严格的单向数据流
---
title: react生命周期
date: 2020-03-31 10:06:14
tags: 
    - react
    - note
---

# React 的生命周期

## React 组件生命周期的执行次数是什么样子的？

- 只执行一次 : `constructor、 componentWillMount、componentDidMount`

- 执行多次 : `render、 子组件的componentWillReceiveProp、componentWillUpdate、componentDidUpdate`

- 有条件执行 : `componentWillUnmount` (页面离开，组件销毁时)

- 不执行的 : 根组件（ReactDOM.render在DOM上的组件）的`componentWillReceiveProps`（因为压根没有父组件给传递props）  

## 执行顺序

假设组件嵌套关系是 App里有parent组件，parent组件有child组件。

如果不涉及`setState`更新

```
    App：   constructor --> componentWillMount -->  render --> 
    parent: constructor --> componentWillMount -->  render --> 
    child:    constructor --> componentWillMount -->  render  --> 
    componentDidMount (child) -->  componentDidMount (parent) --> componentDidMount (App)
```

这时候触发App的setState事件

```
    App：   componentWillUpdate --> render --> 
    parent: componentWillReceiveProps --> componentWillUpdate --> render --> 
    child:    componentWillReceiveProps --> componentWillUpdate --> render -->
    componentDidUpdate (child) -->  componentDidUpdate (parent) --> componentDidUpdate (App)
```

如果触发Parent的setState事件

```
parent： componentWillUpdate --> render --> 
child:     componentWillReceiveProps --> componentWillUpdate --> render --> 
componentDidUpdate (child) -->  componentDidUpdate (parent) 
```

如果只触发child的setState事件

```
child： componentWillUpdate --> render -->  componentDidUpdate (child)
```

这个的执行顺序类似于压栈和出栈


react 生命周期函数
初始化阶段：

getDefaultProps:获取实例的默认属性

getInitialState:获取每个实例的初始化状态

componentWillMount：组件即将被装载、渲染到页面上

render:组件在这里生成虚拟的 DOM 节点

componentDidMount:组件真正在被装载之后

运行中状态：

componentWillReceiveProps:组件将要接收到属性的时候调用

shouldComponentUpdate:组件接受到新属性或者新状态的时候（可以返回 false，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行了）

componentWillUpdate:组件即将更新不能修改属性和状态

render:组件重新描绘

componentDidUpdate:组件已经更新

销毁阶段：

componentWillUnmount:组件即将销毁


## 参考
 - [react的生命周期](https://www.cnblogs.com/soyxiaobi/p/9559117.html)
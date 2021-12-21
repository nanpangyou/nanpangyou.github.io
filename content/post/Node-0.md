---
title: Node-0
categories:
  - Note
  - learnNote
tags:
  - note
  - node
date: 2019-08-23 09:42:47
---

# Node - 0

## 什么是 Node？

维基百科：

> Node.js is an open-source, cross-platform, JavaScript run-time environment that executes JavaScript code outside of a browser.

大意是 Node.js 是一款开源的，跨平台的，能让 JavaScript 在浏览器以外运行的*运行环境*

## 优点

1. 异步非阻塞的 I/O
2. 适用于 I/O 密集型应用
3. 事件循环机制
4. 单线程
5. 跨平台

## 缺点

1. 回调函数较多，嵌套较深
2. 单线程，处理不好 CPU 密集型任务

## Node 的应用场景（大概）

1. Web 服务的 API
2. 服务端渲染
3. 后端的 web 服务，如跨域，服务器端请求

## Node 中函数的特点

我们可以通过 `console.log(arguments.callee.toString())` 来看到整个 Node 是通过一个大函数
`function (exports, require, module, __filename, __dirname) { }`
所包裹的，其中前三个是有关模块化的参数， `__filename` 是指但前文件的绝对路径 `__dirname`是指当前文件夹的绝对路径。

## Node 中的 JavaScript 和浏览器端的 JavaScript 区别

- 浏览器

  1. BOM
  2. DOM
  3. ES 规范

- Node 端
  1. 没有 BOM
  2. 没有 DOM
  3. 几乎包含所有的 ES 规范
  4. 没有 Window 全局变量，取而代之的是 Global

### 需要留意的一个区别

在浏览器下运行 `console.log(this)` 会打印出 Window 对象
但是在 node 环境下 运行 `console.log(this)` 会打印一个空对象 `{}` 如果要打印 global 对象，需要直接使用`console.log(global)`

---
layout: learnnote
title: eventloop
date: 2020-11-13 02:22:19
---

# Eventloop

关于`Eventloop`，大概可以理解为一种操作系统的运行机制。JS使用的就是这种运行机制来解决单线程运行带来的一些问题。

Wiki的定义如下

> "Event Loop是一个程序结构，用于等待和发送消息和事件。（a programming construct that waits for and dispatches events or messages in a program.）"

有关`Eventloop`的详细信息，可以参考Node的官方文档。

这里主要是就常见的题目来进行总结。

---

Eventloop主要有6个阶段
- timers
- I/O callbacks
- idle prepare
- poll
- check
- close callbacks

**牢记以下的三个不同阶段**

- timers阶段
- poll阶段
- check阶段
  
与之对应的有三个常见的API

- setTimeout
- setImmediate
- nextTick

其中 
setTimeout 会在timers阶段执行
setImmediate 会在check阶段执行
nextTick 会在每个阶段结束后立刻执行

举个🌰
```
setImmediate(()=>{
    console.log('setImmediate1')
    setTimeout(()=>{
        console.log('setTimeout1')
    },0)
})

setTimeout(()=>{
    console.log('setTimeout2')
    setImmediate(()=>{
        console.log('setImmediate2')
    })
},0)
```
在这个🌰中
最外层有两个函数，分别为setImmediate和setTimeout。每个函数内部又有一个与之不同的函数
1. 先执行最外层
   > setImmediate将函数体(fn1)内部的部分放入check阶段。setTimeout将函数体(fn2)内部的部分放入timers阶段。

2. eventloop由poll阶段执行到check阶段
    > eventloop由poll阶段执行到check阶段后发现fn1，执行fn1。先输出'setImmediate1'，然后执行fn1内部的setTimeout(fn3)，将fn3的函数体放入timers阶段。

3. eventloop由check阶段执行到timers阶段
   > eventloop由check阶段执行到timers阶段发现fn2，fn3。先执行fn2，输出‘setTimeout2’然后将fn2内部的setImmediate(fn4)放入check阶段，继续执行fn3，输出‘setTimeout1’

4. eventloop由timers阶段执行到poll阶段再到check阶段
   > 发现有fn4，执行后输出'setImmediate2'

所以结果为setImmediate1->setTimeout2->setTimeout1->setImmediate2

# 浏览器中的eventloop

浏览器中没有setImmediate和nextTick

一般将浏览器中的调度分为`宏任务`和`微任务`

一般可以这样理解。

宏任务就是可以一会做的任务，微任务就是要马上做的任务

**常见的浏览器异步中（setTimeout，setInterval，promise）只有promise.then(fn)后面的fn属于微任务，要立马执行，其他均为宏任务**

如果见到await，可以将await改写为promise即可判断。

值得注意的是:**new Promise（fn）后面的函数fn是立即执行的**


举个🌰
```
async function async1(){
    console.log(1)
    await async2();
    console.log(2)
}

async function async2(){
    console.log(3)
}

async1()

new Promise(function(resolve){
    console.log(4)
    resolve()
}).then(function(){
    console.log(5)
})
```
在这个🌰中

1. 声明了两个async函数。执行async1
   > 先输出console.log(1)。然后将await async2()以及后面的部分改写为 async2().then(console.log(2)).这时先执行async2函数，输出console.log(3).然后将then后面的函数放入微任务队列

2. 继续执行后面的函数
   > new Promise内部的函数会立即执行。所以接着输出console.log(4)，然后执行resolve回调。将Promise后面的then函数放入微任务队列

3. 检查微任务队列
   > 按照顺序依次执行。先输出console.log(2).然后输出console.log(5)

最后的结果：1->3->4->2->5
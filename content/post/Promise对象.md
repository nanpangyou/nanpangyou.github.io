---
title: Promise对象
categories:
  - Note
  - daily
tags:
  - note
date: 2019-07-20 10:32:28
---

Promise 是异步编程的一种解决方案。

简单讲 promise 就是一个容器，里面装有一个未来得到结果的函数（通常为异步）。
在 js 中，promise 是以对象的形式存在，提供统一的 API， 对各种一步操作可以使用统一的方法处理。

promise 主要有两个特征。

1. 对象的状态不受外界影响。

   > Promise 对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和 rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。
   > promise 对象的状态改变，只有两种情况： 1. 从 pending 到 fulfiled 2. 从 pending 到 rejected 。只要这两种状态发生后就冻结了，不会再变

## Promise 的基本用法

```
function setTime(tm) {
  return new Promise((resolve, reject) => {
    // setTimeout 的第三个参数是传给回调函数的第一个参数
    setTimeout(resolve, tm, "success");
    // setTimeout(reject, tm, "somethingErr");
  });
}

setTime(4000).then(
  data => {
    console.log("done" + data);
  },
  err => {
    console.log("err" + err);
  }
);

```

**有个注意的地方**

如果函数是这么写的：

```
function setTime(tm) {
  return new Promise((resolve, reject) => {
    // setTimeout 的第三个参数是传给回调函数的第一个参数
    setTimeout(resolve, tm, "success");
    setTimeout(reject, tm, "somethingErr");
    console.log(1)
  });
}

```

那么，`console.log(1)`是会执行的，并且率先打印出来。而处于后一行的 setTimeout 函数则不执行。
这个分开来看

- console.log(1)率先打印出来，是因为在第一轮执行过程中，setTimeout 作为异步函数，会在之后执行结束。而 console.log(1)则会在本轮执行。
- 后一行的 setTimeout 不执行，是因为，在第一行的异步执行过后，promise 的状态就会被冻结，不会再有任何改变。所以如果一个 promise 有多个返回（一般不可能），总会以最快的那个回调函数的结果为准。

## Promise.prototype.then()

`promise.then()` 函数的主要作用，是为了给 promise 的实例添加状态改变时的回调函数。
第一个参数对应了 promise 的 resolve 状态。当异步执行成功后会执行。
第二个参数对应了 promise 的 reject 状态，当异步执行失败后会执行。

`then` 方法返回的是一个新的 promise 实例（不是原来的那个 promise 实例），因此可以使用链式调用。

## Promise.ptototype.catch()

`catch` 方法是 `.then(null, rejection)`或者`.then(undefined, rejection)` 的别名，用于制定发生错误时的回调函数。

Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个 catch 语句捕获。

在日常开发中，建议不用在每一个 then 函数中都指定第二个函数，而是指定一个成功的函数。最后在链式调用的最后用 catch 方法捕获就好

**promise 会吃掉错误**

和`try/catch`不同的是，如果没有使用 catch 捕获错误，Promise 对象抛出的错误也不会传递到外层函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。

一般总是建议，Promise 对象后面要跟 catch 方法，这样可以处理 Promise 内部发生的错误。catch 方法返回的还是一个 Promise 对象，因此后面还可以接着调用 then 方法。

## Promise.prototype.finally()

finally 方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。
finally 方法的回调函数不接受任何参数，这意味着没有办法知道，前面的 Promise 状态到底是 fulfilled 还是 rejected。这表明，finally 方法里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果。

## Promise.prototype.all()

Promise.all 方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

```
const p = Promise.all([p1, p2, p3]);
```

Promise.all 方法接受一个数组作为参数，p1、p2、p3 都是 Promise 实例，如果不是，就会先调用下面讲到的 Promise.resolve 方法，将参数转为 Promise 实例，再进一步处理。（Promise.all 方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。）

p 的状态由 p1、p2、p3 决定，分成两种情况。

（1）只有 p1、p2、p3 的状态都变成 fulfilled，p 的状态才会变成 fulfilled，此时 p1、p2、p3 的返回值组成一个数组，传递给 p 的回调函数。

（2）只要 p1、p2、p3 之中有一个被 rejected，p 的状态就变成 rejected，此时第一个被 reject 的实例的返回值，会传递给 p 的回调函数。

## Promise.prototype.race()

Promise.race 方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```
const p = Promise.race([p1, p2, p3]);
```

只要 p1、p2、p3 之中有一个实例率先改变状态，p 的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给 p 的回调函数。

### 周一快乐

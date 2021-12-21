---
title: 代码片段-js

date: 2019-09-07 16:23:58
---

---

- curry 函数（可以将函数柯里化的函数）
  当传入的参数个数没有到达 func 函数要求的参数个数的时候一直返回柯里化函数。 只有参数满足 func 函数的个数的时候才通过 apply 执行 func 函数

  ```
    /*
      function curry(???){
          ???
          return ???
      }
      var abc = function(a, b, c) {
        return [a, b, c];
      };
    */

    function curry(fn,thisArg){
        if ( !Array.isArray(thisArg) ) {
                  thisArg = [];
        }
        return function(){
              let args = Array.prototype.slice.call(arguments);
              if ( (args.length+thisArg.length) < fn.length ) {
                  return curry(fn , thisArg.concat(args));
              }
              return fn.apply(this , thisArg.concat(args));
          };
    }

    var abc = function(a, b, c) {
      return [a, b, c];
    };

    var curried = curry(abc);

    curried(1)(2)(3);
    curried(1, 2)(3);
    curried(1, 2, 3);
  ```

  **[参考博客](https://yi-love.github.io/articles/js-func-curry)**

---

- 简易双向绑定

  ```
    class X{
        __N行代码__
    }

    var view = new X({
        data: {
            name: 'frank'
        }
    })

    console.log(view.name === 'frank') // 输出 true
    view.name = 'jack' // 输出「有人修改了 name」
  ```

  使用 Object.defineProperty

  ```
  class X{
      constructor(options){
        Object.defineProperty(this, 'name',{
          get(){
              return options.data.name
          },
          set(value){
              options.data.name =  value
              console.log('有人修改了name')
          }
        })
      }
    }
    var view = new X({
        data: {
            name: 'frank'
        }
    })
    console.log(view.name === 'frank') // 输出 true
    view.name = 'jack' // 输出「有人修改了 name」
  ```

  (使用 Proxy 的方式还是不太会)

---

- 事件触发和监听

```
  class EventHub{
    constructor(){
        this.events = {}
    }
    on(eventName,fn){
        console.log('间听到事件',data)
        if(!this.events[eventName]){
            this.envents[eventName] = []
        }
        this.events[eventName].push(fn)
    }
    trigger(eventName,params){
          let fnList = this.events[eventName]
          fnList.map((fn)=>{
              fn.apply(null,params)
          })
    }
  }
  var a = new EventHub()
  a.trigger('zzz','a')
  a.on('zzz',()=>{
      console.log('间听到了')
  })
```

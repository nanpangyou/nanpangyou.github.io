---
title: vue组件之间传参之兄弟组件
categories:
  - Note
  - daily
tags:
  - note
date: 2019-08-19 15:01:08
---

# vue 组件传参之兄弟组件

之前有一篇写了 vue 组件传参，主要写了父子组件之间的传参，通过 `props` 和 `$emit('eventName')` 的方式。

这次主要来写一下另一种情况，两个平级的兄弟组件之间怎么传参。

主要原理和父子组件之间通过触发事件来传参差不多
如何做：

1. 兄弟组件之间的传参需要借助 `eventBus` 这个概念。
2. `eventBus` 其实也是一个 `vue` 实例。
3. 兄弟组件通过引入同一个 `eventBus` 来建立联系，触发并且监听同一个事件来进行传参
4. 数据发送方，通过触发 `bus.$emit('eventName', data)` 来发送数据
5. 数据接收方，通过 `bus.$on('eventName', (data)=>console.log(data))` 其中 \$on 的回调函数就可以接受传参

上 Demo：

### 父组件

```
//  父组件（主要在 Demo 中做包裹层）
<template>
  <div>
    <component-a></component-a>
    <component-b></component-b>
  </div>
</template>

```

### eventBus.js

```
//  eventBus.js( 事件车，主要是用来承载监听函数)
import Vue from 'vue'

export default new Vue();
```

### 组件 A

```
// 组件A  事件的触发方
<template>
  <div>
    <h1>a组件</h1>
    <button @click="abtn">A按钮</button>
  </div>
</template>

<script>
import eventVue from "./eventBus.js";
export default {
  name: "app",
  data() {
    return {
      msg: "我是组件A"
    };
  },
  methods: {
    abtn: function() {
      eventVue.$emit("myFun", this.msg); //$emit这个方法会触发一个事件
    }
  }
};
</script>
```

### 组件 B

```
//  组件B 事件的监听方
<template>
  <div>
    <h1>b组件</h1>
    <div class="components-a">
      <div>{{btext}}</div>
    </div>
  </div>
</template>
<script>
import eventVue from "./eventBus.js";
export default {
  name: "app",
  data() {
    return {
      btext: "我是B组件内容"
    };
  },
  created: function() {
    this.bbtn();
  },
  methods: {
    bbtn: function() {
      eventVue.$on("myFun", message => {
        //注意这里的 this 指向 非箭头函数会有问题
        this.btext = message;
      });
    }
  }
};
</script>
```

效果截图：
![效果截图](siblings-component.gif)

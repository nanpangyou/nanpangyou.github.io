---
title: Vue组件间的传值
categories:
  - Note
  - daily
tags:
  - note
date: 2019-06-18 19:21:50
---

# Vue 组件间的传值

Vue 组件间的传值，说白了就是父子组件和兄弟组件之间的传值。
这次梳理一下父子组件之间的传值。

## Vue 父子组件之间的传值 👨👦

传值主要涉及两种类型的传值

- 单纯的值
- 函数（方法）

> 单纯的值

单纯的值传递，主要是想将父组件`data`对象中的值传入子组件，在子组件中使用。

主要使用 props

```
<div class='app'>
  <!-- 将需要传递的值通过 v-bind 的形式传递给子组件 -->
  <com1 :mymsg='msg' ></com1>
</div>

var app = new Vue({
  el: '.app',
  data: {
    msg: '要传入子组件的值'
  },
  components:{
    com1:{
      template: '<div>{{ mymsg }}</div>',
      <!-- 在组件中使用 props 接受父组件传递过来的值，并且声明，注意： 1，命名要一致，2.props是个数组 -->
      <!-- props中的值和data中的值都可以使用，区别不大，但是要注意，父组件传递过来的值不要重新赋值，赋值只会在子组件内部生效 -->
      props: ['mymsg']
    }
  }
})

```

> 函数传递

父组件的函数传递给子组件，一般用在子组件内部更新后让父组件刷新状态。

主要使用 this.\$emit()

```
<div class='app'>
  <!-- 将需要传递的值通过 v-on 的形式传递给子组件 -->
  <com1 @func='show' ></com1>
</div>

var app = new Vue({
  el: '.app',
  data: {
    msg: '要传入子组件的值'
  },
  methods: {
    show(arg){
      // 如果在子组件的this.$emit('func')有第二，第三个参数的时候，这里的，arg对应第二个参数，依次往后，就可以拿到子组件的值了
      console.log('父组件的show方法')
    }
  }
  components:{
    com1:{
      template: '<input type="button" @click='subshow' value='点击调用父组件的方法'>',
      methods: {
        subshow(){
          //如果需要向父组件传递值，则只需要在this.$emit方法中增加第二第三个参数
          this.$emit('func',data)
        }
      }
    }
  }
})
```

## 在父组件中直接获取子组件的数值或者函数

在 Vue 中，可以使用 ref 来做到类似上古时代使用 id 绑定元素的功能

```
<div class= 'app'>
  <p ref='p'>p里面的数据</p>
  <com1 ref='com1'></com1>
</div>

let app = new Vue({
  el: '.app',
  data: {},
  components: {
    com1: {
      template: `<div>子组件</div>`,
      data(){
        return {
          submsg: '子组件中的值'
        }
      },
      methods: {
        subshow(){
          console.log("执行子组件的show方法")
        }
      }
    }
  }
})


就可以使用 app.$refs.p 来获取原生的dom元素p。来用相关的原生api

使用 app.$refs.com1 来获取子组件com1，可以使用app.$refs.com1.submsg 来获取子组件的data值，
使用 app.$refs.com1.subshow() 来执行子组件的subshow 方法


```

羊肉夹饼 GOGOGO！🐑🐑🐑🐑🐑🐑🐑🐑🐑🐑

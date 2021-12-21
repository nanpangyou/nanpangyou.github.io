---
layout: learnnote
title: Vue——transition动画的一个小注意点
date: 2019-05-30 15:43:14
---
# 太长不看就记住`display`属性不能`transition`

起源于日常进行着龟速的Vue自啃。

看到有关于`transition`标签相关的内容。
做了一个小小的动画实验。

Demo：
```
<body>
    <div class="app">
      <!-- 测试小球的动画，使用Vue自带的transition元素 -->
      <button @click="flag = !flag">车来了</button>
      <transition
        @before-enter="beforeEnter"
        @enter="enter"
        @after-enter="afterEnter"
      >
        <div class="car" v-show="flag">车速很快</div>
      </transition>
    </div>
    <script>
      var app = new Vue({
        el: ".app",
        data: {
          flag: false
        },
        methods: {
          beforeEnter(el) {
            el.style.transform = "translate(0,0)";
          },
          enter(el) {
            el.style.transform = "translate(350px,0)";
            el.style.transition = "all 400ms ease";
          },
          afterEnter(el) {
            this.flag = !this.flag;
          }
        }
      });
    </script>
  </body>
```
这个demo中在transition中使用了自带的钩子函数，分别处理动画开始前（before-enter）,动画开始后到动画结束（enter），以及动画结束后（after-enter）三个不通状态的样式

在上面的代码里，虽然在`enter`的方法中使用了 `el.style.transition = 'all 400ms ease'`,但是，代表🚗的方块并没有从开始位置逐渐过度到结束的位置。只是在结束的位置瞬间出现。

网上查了一圈后，流行的说法是因为`transition`属性不支持`display`,也就是说`display`这个属性不可以发生渐变。
本例中，小🚗通过`data`中的`flag`来控制🚗的显示和隐藏。当动画开始的时候，🚗还是`display:none;`的状态，又由于`display`属性不可渐变。所以只有当移动到结束位置的时候才触发重绘，将🚗显示出来。

解决方法就是要在🚗移动之前就触发浏览器的重绘，将🚗显示出来，这样，🚗的位移就可以渐变了。

大多数此类的代码触发重绘的属性是`offsetWidth`或者`offsetHeight`之类的。（offset家族）

所以改造后的代码如下
demo2:
```
 <body>
    <div class="app">
      <!-- 测试小球的动画，使用Vue自带的transition元素 -->
      <button @click="flag = !flag">车来了</button>
      <transition
        @before-enter="beforeEnter"
        @enter="enter"
        @after-enter="afterEnter"
      >
        <div class="car" v-show="flag">车速很快</div>
      </transition>
    </div>
    <script>
      var app = new Vue({
        el: ".app",
        data: {
          flag: false
        },
        methods: {
          beforeEnter(el) {
            el.style.transform = "translate(0,0)";
          },
          enter(el, done) {
            el.offsetWidth;
            el.style.transform = "translate(350px,0)";
            el.style.transition = "all 400ms ease";
            done();
          <!-- done 方法用来立即触发afterEnter函数，换句话说就是after-enter钩子函数的别名。 -->
          <!-- 如果不立即调用done函数，则会有少许延迟才调用after-enter函数 -->
          },
          afterEnter(el) {
            this.flag = !this.flag;
          }
        }
      });
    </script>
  </body>
```
这样，整个的动画就稍显流畅了。



资料：
[有关网页呈现，重绘，回流](https://blog.csdn.net/xingsilong/article/details/80624765)
最后的展示：
[小球动画demo](/demo/小球动画.html)

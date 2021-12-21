---
layout: learnnote
title: "style,getComputedStyle,getBoundingClientRect的区别"
date: 2020-01-17 15:34:21
---

今天在做 toast 组件的时候，发现了一个问题
大致过程如下
在做 toast 组件的时候，想支持用户自己传递 CloseButton,在 CloseButton 和 toast 主体之间做一个竖线进行分隔

由于父元素的高度本身设置的为`min-height`；
子元素设置`height:100%;`就完全无效

所以只能在`mounted`生命周期内用 js 进行赋值

设置了 refs => 通过 vm.\$refs.line.style.height 取值
发现取不到
只能通过 vm.\$refs.toast.getBoundingClientRect().height 才可以取到高度

然后使用
`vm.\$nextTick()`api 来设置高度。

第一种方法取不到的原因是，style 只能取到行内样式，也就是说，如果你不是在标签内写 style 样式，都不能通过 style 来取到

查询 api 后使用`getBoundingClientRect`api 取到了高度

参考资料：
[js 中 style,currentStyle,getComputedStyle 和 getBoundingClientRect 的区别以及获取 css 操作方法](https://blog.csdn.net/qishuixian/article/details/79458483)
[JavaScript 中 getBoundingClientRect()方法详解](https://www.cnblogs.com/libin-1/p/6137167.html)

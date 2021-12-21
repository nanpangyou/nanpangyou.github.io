---
title: AsyncComponent
date: 2021-07-07 15:41:54
---
# Vue中的异步组件

当我们使用Vue时，文件会被默认打包成一个chunk文件，也就是说当我们页面初次加载的时候，浏览器会下载全量的js文件。有时文件会比较大，所以有一种优化方式是使用异步组件，使得当用户使用某一组件时才会去下载。

> Demo：


![AsyncComponent.png](AsyncComponent.png)


> 当点击按钮时，浏览器会去下载list组件的代码，在过程中会显示loading组件，下载的包的名字是在注释中 `webpackChunkName` 规定的


详情可以参考官方文档： [异步组件文档](https://cn.vuejs.org/v2/guide/components-dynamic-async.html)
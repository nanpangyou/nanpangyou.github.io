---
title: VueRouter在Webpack上的坑
categories:
  - Note
  - daily
tags:
  - note
date: 2019-08-02 17:48:53
---

# VueRouter 学习笔记之在 Webpack(4.X) 上的坑

通常 VueRouter 给我们提供了常用的两种方式的使用

* hash 模式
  在 url 中有 # 号的存在，不美观，也不利于 SEO ，部署没有成本，用户向服务器发送请求时，浏览器会忽略 # 后面的值。刷新时不会重新请求。

* history 模式
  url 看起来是正常的，会向服务器发送请求。所以在刷新的时候，会向服务器发送对应路径的请求。服务器没有对应的路径，就会报404


解决办法，就是在 webpack 的配置项中，在 devServer 加上

```
//history模式下的url会请求到服务器端，但是服务器端并没有这一个资源文件，就会返回404，所以需要配置这一项
historyApiFallback: {
            rewrites: [{
                from: /.*/g,
                to: '/index.html' //与output的publicPath有关(HTMLplugin生成的html默认为index.html)
            }]
        }
```
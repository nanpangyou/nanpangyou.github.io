---
title: url编码
categories:
  - Note
  - daily
tags:
  - note
date: 2019-08-13 15:39:32
---

# encodeURI() 和 encodeURIComponent() 方法

encodeURI() 和 encodeURIComponent() 方法，通常都用来进行对 url 的编码。

- 为什么要编码：
  标准的 url 中不可以出现特殊字符，如 空格 等，如果有则会造成浏览器无法正确理解 url，因此需要使用 utf-8 来对特殊的字符进行替换

- 区别：
  encodeURI() 方法常用来对整个 URL 进行编码，但是对本身属性 url 的特殊字符进行编码，例如“冒号、正斜杠、问号、井号”则不转换。
  encodeURIComponent() 方法常用域对 url 中的某一块进行编码，一般是参数部分，他会将任何非标准字符进行编码。

- 对应的解码函数：
  encodeURI() => decodeURI()
  encodeURIComponent() => decodeURIComponent()

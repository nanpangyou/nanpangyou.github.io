---
title: 使用npm安装时的参数

date: 2019-06-28 15:02:12
---

日常使用`npm i xxxx` 和 `npm i xxx -D`还有`npm i xxx -S`傻傻分不清。

`npm i xxx -D`等同于`npm install xxx --save-dev`，它所安装的文件会将版本信息和名字写入`package.json`中的`devDependencies`对象中。

`npm i xxx -S`等同于`npm install xxx --save`,它所安装的文件会将版本信息和名字写入`package.json`中的`dependencies`对象中。

那么问题来了，`devDependencies`和`dependencies`有什么区别？

> devDependencies

主要是开发时所依赖的包，这个目录下的不需要部署到生产

> dependencies

项目运行所依赖的包，需要在生产也使用

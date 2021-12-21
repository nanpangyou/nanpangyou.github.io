---
title: Node-4
categories:
  - Note
  - learnNote
tags:
  - note
  - node
date: 2019-08-29 11:03:50
---

# Node 原生服务器搭建(简单版)

```
// 引入http模块
let http = require("http");
// 引入查询字符串处理函数
let { parse } = require("querystring");

// 创建server对象
let server = http.createServer((request, response) => {
  // request 请求对象
  // response 返回对象
  console.log(request.url);
  let str = request.url.split("?")[1];
  let obj = parse(str);
  console.log(obj);
  response.setHeader("content-type", "text/html;charset=utf-8");
  if (obj.haha === "1") {
    response.end("h1");
  } else {
    response.end("<h1>服务器启动成功</h1>");
  }
});
// 监听端口
server.listen(9999, err => {
  if (!err) {
    console.log("服务器启动成功");
  } else {
    console.log("服务器启动失败");
  }
});

```

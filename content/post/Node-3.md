---
title: Node-3
categories:
  - Note
  - learnNote
tags:
  - note
  - node
date: 2019-08-26 15:30:49
---

# mongoose

## 一个概念

非关系型数据库：对象文档模型（ODM）库
关系型数据库：对象关系模型（ORM）映射表

## Mongoose

Mongoose 是一个对象文档模型（ODM）库，它对 Node 原生的 MongoDB 模块进行了优化封装，并提供了很多功能。

优点：

1. 可以为文档创建一个模式结构（Schema）
2. 可以对模型中的对象/文档进行验证
3. 数据可以通过类型转换转换为对象模型
4. 可以使用中间件来应用业务逻辑挂钩
5. 比 Node 原生的 MongoDB 驱动更容易

明确几个概念

> mongoDB: 一个非关系型数据库的名称
> mongod: 启动 mongo 服务的命令
> mongo: 连接数据库的命令
> mongoose: 在 Node 端连接数据库的一个库

## Mongoose 的 CRUD

- Create

  模型对象.create(文档对象,回调函数)
  模型对象.create(文档对象) //返回值是一个 Promise 对象

- Read

  模型对象.find(查询条件[,投影]) //不管有没有数据，都返回一个数组
  模型对象.findOne(查询条件[,投影]) //找到了返回一个对象，没找到返回 NULL

* Update

  模型对象.updateOne(查询条件,要更新的内容[,配置对象])
  模型对象.updateMany(查询条件,要更新的内容[,配置对象])
  备注：存在 update 方法，但是即将废弃，查询条件匹配多个时，只会更新一个，建议使用 updateOne()

* Delete

  模型对象.deleteOne(查询条件)
  模型对象.deleteMany(查询条件)
  备注： 没有 delete 方法，会报错

## Schema

类似于建表语句，用来约束字段类型以及是否必须等等

```
let mongoose = require("mongoose");
//  引入约束schema
let Schema = mongoose.Schema;
// 创建一个约束对象实例
let studentSchema = new Schema({
  stu_id: {
    type: String,
    required: true,
    unique: true
  },
  name: {
    type: String,
    required: true
  },
  age: {
    type: Number,
    required: true
  },
  sex: {
    type: String,
    required: true
  },
  hobby: [String],
  info: {
    type: Schema.Types.Mixed
  },
  date: {
    type: Date,
    default: Date.now()
  },
  enable_flag: {
    type: String,
    default: "Y"
  }
});
// 创建模型对象
module.exports = mongoose.model("student", studentSchema); //第一个参数与数据库中的集合相对应，第二个参数制定约束对象实例

```

## Demo

```
let studentDB = require("./db/studentsDB");
let studentSchema = require("./models/studentSchema");
!(async () => {
  await studentDB;
  let insertResult = await studentSchema.create({
    stu_id: "201908272007",
    name: "lee",
    age: 18,
    sex: "男",
    hobby: ["吃饭", "逛街"],
    info: 7777
  });
  console.log(insertResult);
})();

```

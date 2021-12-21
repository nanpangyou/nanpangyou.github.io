---
title: Node-2
categories:
  - Note
  - learnNote
tags:
  - note
  - node
date: 2019-08-26 11:00:03
---

# Mongodb

## 数据库分类

1. 关系型数据库（RDBS）
   如 MySQL, Oracle, DB2, SQL server ...

优点 ：

- 易于维护： 都是使用表结构，格式一致
- 使用方便： SQL 语言通用，可用于复杂查询
- 高级查询： 可用于多表之间的复杂查询

缺点 ：

- 读写性能比较差，尤其是海量数据的高效率读写
- 有固定的表结构，字段不可以随意修改，灵活度欠缺
- 高并发的读写需求，传统关系行数据库来说， 硬盘 I/O 是一个很大的瓶颈

2. 非关系型数据库
   如 MongoDB, Redis ...

优点 ：

- 格式灵活：存储数据的格式可以是 key value 格式
- 速度快：nosql 可以内存作为载体，而关系型数据库只能用硬盘
- 易用： nosql 部署简单

缺点 ：

- 不支持 sql
- 不支持事务

### 关键字

| 关系型数据库 | 非关系型数据库 |
| ------------ | -------------- |
| 数据库       | 数据库         |
| 表           | 集合           |
| 每一条数据   | 文档           |

## MongoDB 安装后的一些注意

1. 使用 homebrew 安装 MongoDB

```
brew install mongodb
```

2. 安装成功后可使用 `mongod -version` 来查看是否成功

3. 然后需要新建数据文件夹

```
sudo mkdir -p /data/db
```

4. 唤醒 MongoDB

```
mongod
```

5. 另打开一个终端 输入 `mongo` 进入命令状态操作数据库
6. 结束后，输入以下命令关闭数据库

```
use admin;

db.shutdownServer();
```

## MongoDB 的原生 CRUD（增删改查）

### -C creat：

- db.集合名.insert(文档对象)
- db.集合名.insertOne(文档对象)
- db.集合名.insertMany([文档对象，文档对象])

### -R read:

- db.集合名.find(查询条件[,投影])
  例如： db.student.find({age:18}),查找年龄为 18 的数据。 db.student.find({age:18,name:'jack'}),查找年龄为 18，姓名为 jack 的数据

  常用操作符：

  1. \> , <, >=, <=, !== 对应为： \$gt, \$lt, \$gte, \$lte, \$ne
     例如：db.student.find({age:{\$gt:18}})
  2. 逻辑或： 使用\$in 或 \$or
     查找年龄为 18 或者 20 的学生
     db.student.find({age: {\$in:[18,20]}})
     db.student.find({\$or:[{age:18},{age:20}])
  3. 逻辑非： \$nin (not in)
  4. 正则匹配：
     举例： db.student.find({name:/^T/)
  5. \$where 能写函数
     举例：
     db.student.find({\$where:function(){
     return this.name === 'jack' && this.age === 18  
     }})

  投影：
  db.studnet.find({age:18},{name:1}) //查询所有 age 为 18 的 name 字段
  db.studnet.find({age:18},{sex:0}) //查询所有 age 为 18 的 数据，不要 sex 字段  
  投影的 0 和 1 的约束条件不能混写

### -U update

- db.集合名.update(查询条件, 要更新的内容[, 配置对象])

  例如：
  //如下会将更新内容替换掉整个的文档对象，但 \_id 不受影响
  db.student.update({name:'jack'},{age: 19}) //把 jack 的年龄改为 19，其他字段会置空

  // 使用 \$set 修改制定内容，其他内容不变，不过只能匹配一个 name 为 jack 的数据
  db.student.update({name:'jack'},{\$set: {age: 19} })

  // 修改多个对象，匹配多个 jack 把所有 jack 的年龄改为 19
  db.student.upadate({name: 'jack'},{\$set:{age: 19}}, {multi: true})

- db.集合名.updateOne(文档对象)
- db.集合名.updateMany([文档对象，文档对象])

### -D delete

- db.集合名.remove(查询条件)
  // 删除所有年龄小于 19 的
  db.student.remove({ age: {\$lt:19} })

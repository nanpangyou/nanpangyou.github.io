---
title: "Mysql Cheat Book"
date: 2022-09-23T11:38:07+08:00
categories:
  - Note
  - daily
tags:
  - Mysql
---

## Mysql cheat book

1. 链接数据库退出链接

```shell

mysql -u root -p

# 或者显示输入密码

mysql -u root -p 123456

# 退出

exit

```

```sql

update mysql.user set authentication_string=password('123456') where user='root';  --修改密码

flush privileges;  --刷新权限

-------------------------------------------

-- 所有的语句要以分号结尾

show databases; --查看所有数据库

use 数据库名; --切换数据库

show tables; --查看当前数据库的所有表

describe 表名; --查看表结构

create database 数据库名; --创建数据库

drop database 数据库名; --删除数据库

```

2. 一些名词

| 名词 | 说明 |
| --- | --- |
|DDL|数据定义语言，用于定义数据库对象，如表、索引、视图等|
|DML|数据操作语言，用于对数据库中的表进行增删改查|
|DCL|数据控制语言，用于对数据库的访问权限进行控制|
|TCL|事务控制语言，用于对事务进行控制|

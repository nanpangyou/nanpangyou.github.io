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

3. 操作数据库

注意： **mysql中的关键字不区分大小写**

3.1 创建数据库

```sql
create Database [if not exists] 数据库名;
```

3.2 删除数据库

```sql
drop database [if exists] 数据库名;
```

3.3 使用数据库

```sql
use `数据库名`;

select `user` from student    -- 使用反引号可以避免关键字冲突

```

3.4 查看所有的数据库

```sql
show databases;
```

4. 数据库的列类型

4.1 数值

* tinyint：1字节，-128~127(十分小的数据)
* smallint：2字节，-32768~32767(较小数据)
* mediumint：3字节，-8388608~8388607(中等数据)
* int: 4字节，-2147483648~2147483647(一般数据)(常用)
* bigint：8字节，-9223372036854775808~9223372036854775807(大数据)
* float：4字节，单精度浮点数
* double：8字节，双精度浮点数
* decimal：可变长度，精确的小数(字符串形式的浮点数， 金融计算用)

4.2 字符串

* char：固定长度，不足的用空格补齐 0-255
* verchar：可变长度，最大长度为65535(常用)
* tinytext：最大长度为255(微型文本)
* text：最大长度为65535(文本)

4.3 日期时间

* date：日期，格式为：YYYY-MM-DD
* time：时间，格式为：HH:MM:SS
* datetime：日期时间，格式为：YYYY-MM-DD HH:MM:SS(常用)
* timestamp：时间戳, 1970-01-01 00:00:00到现在的毫秒数(常用)
* year：年，格式为：YYYY

4.4 null

* 没有值，未知
* 不要使用Null来表示空字符串，应该使用空字符串

5. 数据库的字段属性

### Unsigned

* 无符号，只能存储正数
* 声明了该列不能为负数

### zerofill

* 用0填充，不足的用0补齐
* 用于数值类型
  
### auto_increment

* 自增，每次插入数据时，自动加1
* 通常用于主键
* 可以自定义主键的起始值和步长

### NULL & not NULL

* NULL：可以为空, 默认值为NULL
* not NULL：不可以为空

### default

* 默认值

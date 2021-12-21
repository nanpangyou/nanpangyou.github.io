---
title: Node-1
categories:
  - Note
  - learnNote
tags:
  - note
  - node
date: 2019-08-23 15:59:19
---

# Node - 1

## Buffer 缓冲器

什么是 Buffer ?

1. 它是一个类似于数组的对象，用于存储数据（存储的是二进制数据）
2. Buffer 的效率很高，存储和读取的速度很快，直接对计算机的内存进行操作。
3. Buffer 的大小一旦确定了，就不可修改
4. 每个元素占用内存的大小为 1 字节
5. Buffer 是 Node 中非常核心的模块，无需下载和引用就可以直接使用

使用示例

```
1. Buffer.from()
  // 将一个字符串存入Buffer中
    let str = 'hello world'
    let buf = Buffer.from(str)
    console.log(buf)

2. Buffer.alloc();
  //alloc()方法创建Buffer的效率一般 会开辟一块完全没有使用过的空间
    let buf2 = Buffer.alloc();
    console.log(buf2)

3. Buffer.allocUnsafe();
  //allocUnsafe()方法创建Buffer的效率很好，但是会有一些安全问题 会返回一块没有被引用的空间（有可能会有其他方法不用的数据，数据可能会有安全问题）
    let buf3 = Buffer.allocUnsafe()
    console.log(buf3)

4. new Buffer()
  // 官方不推荐，说是在后续会删除，效率也不高
    let buf4 = new Buffer()
    console.log(buf4)
```

## Node 文件系统

Node 中的文件系统：

1. 在 Node 中有一个文件系统，所谓文件系统，就是对计算机中的文件进行增删查改等操作
2. 在 Node 中，提供了一个模块，叫 fs 模块，专门由于用户操作文件
3. fs 模块，是 Node 的核心模块，使用的时候，要引入，无需下载

```
let fs = require('fs')
```

### 简单文件写入

语法：`fs.writeFile(file, data[,options], callback)`

参数：
--file ： 文件名+文件路径
--data ：要写入的数据
--options ： 配置选项

> --flag : 打开文件要进行的操作 'w':写入(默认) 'a':追加
> --mode : 文件权限的限制 （默认是 0o666: 文件可读可写 0o222+0o444） 0o111:文件可被执行 0o222:文件可被读取 0o444:文件可被写入
> --encoding : utf8(默认值)

--callback ： 回调函数

```
const fs = require('fs')
fs.writeFile('./test.md','test content',(err)=>{
  if(!err){
    console.log('success');
  }else{
    console.log(err);
  }
})
```

#### 简单文件写入的不足

简单文件写入是一次性将所有要写入的数据加载到内存中，对于比较大的数据容易造成内存溢出。适用于较小的文件写入。

### 流式文件写入

流，分为 可读流，可写流

1. 创建一个可写流

`fs.createWriteStream(path[, options])`
--path: 文件路径+文件名
--options: 配置对象（可选）

> flags : 打开文件要进行的操作 'w':写入(默认) 'a':追加
> mode : 文件权限的限制 （默认是 0o666: 文件可读可写 0o222+0o444） 0o111:文件可被执行 0o222:文件可被读取 0o444:文件可被写入
> encoding : utf8（默认）
> fd : 文件的唯一标识
> autoClose : 当数据操作完毕后，自动关闭文件，默认值是 true
> start : 文件写入的其实位置 文件中的位置，不是内存中的位置

```
const fs = require('fs')
let ws = fs.createWriteStream('./demo.md')
// 只要使用了流，需要给流加监听，来控制打开和关闭。
ws.on("open",()=>{
  console.log('流打开了')
})
ws.on("close",()=>{
  console.log("流关闭了")
})

ws.write('马上上学了，\n')
ws.write('饿了，\n')
ws.write('忍着  \n')
// ws.close()  //如果用的是8版本以下（包括8版本），用close方法关闭流，会造成数据的丢失，这个时候推荐使用end方法
ws.end()
```

### 简单文件读取

`fs.readFile(path[, options], callback)`

--path: 文件路径和文件名字
--options： 配置对象

> flag: 'a' add 加入 ， 'w' write 写入 , 'r' read 读取
> encoding: utf8 （默认）

--callback: 回调函数

> err: 错误对象
> data ： 数据

```
const fs = require('fs')
fs.readFile('./demo.md',(err,data)=>{
  if(!err){
    console.log(data)
    //打印的是buffer类型的数据，如果本身文件是文本，可以调用Buffer自身的toString()方法，将buffer类型的数据转换成文本
    //如果本身文件为非文本（例如：mp3 mp4 rar等其他类型），Buffer格式的数据较为方便操作
  }else{
    console.log(err)
  }
})
```

### 流式文件读取

`fs.createReadStream(path[, options])`

--path: 文件路径 + 文件名
--options: 配置文件

> start : 读取的起始点
> end : 读取的结束点
> highWaterMark ：每次读取的数据大小 默认值是 64\* 1024 （65536） kb

```
const fs = require('fs')
// 创建一个可读流

let rs = fs.createReadStream('./demo.md')
let ws = fs.createWriteStream('./demo-out.md')

//  监听读取流
rs.on('open',()=>{
  console.log('开始读取')
})
rs.on('close',()=>{
  console.log('读取结束')
  ws.end()
})
//  监听写入流
ws.on('open',()=>{
  console.log('写入开始')
})
ws.on('close',()=>{
  console.log('读取结束')
})

// 当给可读流绑定一个data事件，会自动触发流读取文件
rs.on('data',data=>{
  console.log(data)   // 读取的数据
  console.log(data.toString())  // 读取的Buffer字符串化
  console.log(data.length)  // 读取的Buffer长度
  ws.write(data)

})
```

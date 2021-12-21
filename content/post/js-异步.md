---
title: js-异步
categories:
  - Note
  - js
  - 函数
tags:
  - blog
  - js
  - js深入
date: 2019-09-09 11:04:01
---

# 同步和异步

同步： 等待结果
异步： 不等待结果

异步通常和回调一起出现，但是要注意，异步不是回调，回调也不一定是异步。

## 常见的异步代码

1. 获取图片的宽高

```
  var w = document.getElementByTagName('img')[0].width
  console.log(w)  //0
```

解决办法：

使用 img 的 onload(表示图片已经下载并加载完成)事件来重新触发获取宽度的函数

```
  var img = document.getElementByTagName('img')[0]
  img.onload = function(){
    var w = img.width
    console.log(w)
  }
```

2. 事件绑定的异步

```
  let liList = document.querySelectorAll('li')
  for ( var i = 0;i < liList.length; i++ ){
    liList[i].on('click',function(){
      console.log(i)
    })
  }
```

解决办法：

将循环中变量的关键字 `i` 的声明关键字改为 `let`

```
  let liList = document.querySelectorAll('li')
  for ( let  i = 0;i < liList.length; i++ ){
    liList[i].on('click',function(){
      console.log(i)
    })
  }
```

3. AJAX

```
  let request = $.ajax({
    url: '.',
    async: false
  })
  console.log(request.responseText)
```

解决办法：

AJAX 默认一定是异步的

```
$.ajax({
    url: '/',
    async: true,
    success: function(responseText){
        console.log(responseText)
    }
})
```

## 异步的形式

一般有两种方式拿到异步的结果

1. 轮询
2. 回调函数

## 回调的形式

1. Node.js 的 error-first 形式

```
  fs.readfile('./text.md',(err, data)=>{
    if(err){
      // 失败
      console.log(err)
    }else{
      // 成功
      console.log('文件读取成功', data)
    }
  })
```

2. jQuery 的 success/error 模式

```
$.ajax({
  url: '/xxx',
  success: ()=>{},
  error: ()=>{}
})
```

3. jQuery 的 done / fail / always 模式

```
$.ajax({
  url: '/xxx'
}).done( ()=>{} ).fail( ()=>{} ).always( ()=>{} )
```

4. Promise 的 then 形式

```
$.ajax({
  url: '/xxxx'
}).then( ()=>{}, ()=>{} )
  .then( ()=>{}  )
```

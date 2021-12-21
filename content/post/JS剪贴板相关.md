---
title: JS剪贴板相关
categories:
  - Note
  - daily
tags:
  - note
date: 2019-04-19 16:47:55
---

# JS 剪贴板相关的内容

这两天做了一个需求，是通过 js 获取剪贴板中的字符串，对用户复制不同长度的字符串进行分类处理。

由于自己之前从来都没有关注过复制事件 `paste` ,于是就去网上找了找相关的资料，记录如下：

## JS 是可以拿到用户所输入的内容的

`paste` 事件不一定需要绑定在 `input` 元素中，可以绑定在任意元素中，当用户鼠标在该元素上或者该元素处于 `focus` 状态，绑定的 `paste` 事件就会在复制的时候执行。

## 事件对象

```
        if (window.clipboardData && window.clipboardData.getData) {
          // IE
          pastedText = window.clipboardData.getData("Text");
        } else {
          //e.clipboardData.getData('text/plain');
          pastedText = e.originalEvent.clipboardData.getData("Text");
        }

```

粘贴事件为你提供了一个 `clipboardData` 对象, 它实际上是一个 `DataTransfer` 类型的对象, `DataTransfer` 是拖动产生的一个对象，但实际上粘贴事件也是它。

`clipboardData` 属性介绍

| 属性          | 类型                 | 说明                                          |
| ------------- | -------------------- | --------------------------------------------- |
| dropEffect    | String               | 默认是 none                                   |
| effectAllowed | String               | 默认是 uninitialized                          |
| files         | FileList             | 粘贴操作为空 List                             |
| items         | DataTransferItemList | 剪切板中的各项数据                            |
| types         | Array                | 剪切板中的数据类型 该属性在 Safari 下比较混乱 |

### items 介绍

`items` 是一个 `DataTransferItemList` 对象，自然里面都是 `DataTransferItem` 类型的数据了。

#### 属性

`items` 的 `DataTransferItem` 有两个属性 `kind` 和 `type`

| 属性 | 说明                                                                     |
| ---- | ------------------------------------------------------------------------ |
| kind | 一般为 string 或者 file                                                  |
| type | 具体的数据类型，例如具体是哪种类型字符串或者哪种类型的文件，即 MIME-Type |

#### 方法

| 方法        | 参数     | 说明                                                                                                              |
| ----------- | -------- | ----------------------------------------------------------------------------------------------------------------- |
| getAsFile   | 空       | 如果 kind 是 file，可以用该方法获取到文件                                                                         |
| getAsString | 回调函数 | 如果 kind 是 string，可以用该方法获取到字符串，字符串需要用回调函数得到，回调函数的第一个参数就是剪切板中的字符串 |

在原型上还有一些其他方法，不过在处理剪切板操作的时候一般用不到了。

`types` 介绍
一般 `types` 中常见的值有 `text/plain`、`text/html`、`Files`。

| 值         | 说明                     |
| ---------- | ------------------------ |
| text/plain | 普通字符串               |
| text/html  | 带有样式的 html          |
| Files      | 文件(例如剪切板中的数据) |

**一个 Demo**

```
pasteEle.addEventListener("paste", function (e){
    if ( !(e.clipboardData && e.clipboardData.items) ) {
        return ;
    }

    for (var i = 0, len = e.clipboardData.items.length; i < len; i++) {
        var item = e.clipboardData.items[i];

        if (item.kind === "string") {
            item.getAsString(function (str) {
                console.log(str)
                // str 是获取到的字符串
            })
        } else if (item.kind === "file") {
            var pasteFile = item.getAsFile();
            console.log(pasteFile)
            // pasteFile就是获取到的文件
        }
    }
});
```

注意如果是 `string` 类型的数据，可能针对具体是 `text/plain`、`text/html` 进行分别的处理。

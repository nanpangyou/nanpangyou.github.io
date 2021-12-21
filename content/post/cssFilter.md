---
title: cssFilter
categories:
  - Note
  - daily
tags:
  - note
date: 2019-04-03 16:42:23
---

昨天在写一个需求的时候，用到半透明蒙版遮罩
之前这种需求都是用`background: rgba(0,0,0,.5)`或者`hsla(50, 33%, 25%, 0.75)`这类使用`Alpha`通道,或者使用`opacity`来增加背景层的透明度。

单单增加透明度，整体缺少质感，并且如果有文字在遮罩层上的话会使得上下文字重叠，视觉效果并不好。

毛玻璃的效果刚好可以弥补这一点。

主要用到的属性是`filter: blur(10px);`

代码结构不难理解

```
<body>
    <div class="bg">
      <div class="wrapper">
        <p>
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Alias
          aperiam, sequi velit dolores accusantium voluptas. Facere dolorem
          tempore similique nobis eos maxime error perspiciatis temporibus,
          fugiat vel deserunt esse unde!
        </p>
        <p>
          Lorem ipsum dolor, sit amet consectetur adipisicing elit. Alias
          aperiam, sequi velit dolores accusantium voluptas. Facere dolorem
          tempore similique nobis eos maxime error perspiciatis temporibus,
          fugiat vel deserunt esse unde!
        </p>
      </div>
    </div>
  </body>
```

对应的样式文件

```
 <style>
      * {
        margin: 0;
        padding: 0;
      }
      html,
      body {
        height: 100%;
      }
      .bg {
        background: url(./01.jpeg);
        background-size: cover;
        background-position: center;
        width: 100%;
        height: 100%;
        position: relative;
      }
      .wrapper {
        width: 55%;
        height: 55%;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        overflow: hidden;
      }
      .wrapper:before {
        content: "";
        background: url(./01.jpeg);
        background-position: center;
        background-size: cover;
        position: absolute;
        top: -41%;
        left: -41%;
        bottom: 0;
        right: 0;
        z-index: -1;
        width: 182%;
        height: 182%;
        filter: blur(10px);
      }
      .wrapper > p {
        font-size: 20px;
        color: #fff;
        width: 90%;
        margin: 0 auto;
      }
    </style>
```

最后的效果图如下
![毛玻璃效果](01.png)

值得注意的有以下几点

1. wrapper 层的背景图片和 bg 层的图片是一样的，原理就是两张图片完全重合，设置`overflow: hidden;`来隐藏超出部分并增加模糊
2. 位置计算很重要，要让两张图片重叠，否则模糊部分的纹路对不上背景图片
3. 可以不使用伪元素，一个空的`div`也可以，但是。。。。emmmmm，你懂得

最后一个事，`filter`的兼容性，caniuse 上好好看看呀

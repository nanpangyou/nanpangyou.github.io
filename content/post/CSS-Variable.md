---
layout: learnnote
title: CSS-Variable
date: 2019-11-05 10:38:24
---
# CSS变量

## 语法：

定义变量： `--variableName`
调用变量： `var(--variableName)`
默认值：   `var(--variableName, defaultValue)`
    1. 当 --variableName 不存在的时候，将会使用默认值
    2. 当浏览器不支持时，也应该使用默认值
    3. 变量名区分大小写

```
.className{
    --fontcolor: red;
}
.className2{
    color: var(--fontcolor)
}
.className3{
    color: var(--fontcolor, yellow)
}
```

## 层级

你创建的变量可以在该元素本身以及该元素的子元素内使用

根元素
```
:root{
    --fontcolor: red;
}
```
在根元素定义的变量，则在全局都可以使用`fontcolor`变量

## 优先级

在元素内创建变量
```
.app{
    --fontcolor: blue;
}
```
`:root`内定义的变量是整个页面的，如果在元素内定义同样的变量，则会覆盖全局变量（就近原则）

## 媒体查询

当屏幕的宽度小于等于350px的时候，
--fontcolor的值会被改变为green，那么应用了该变量的元素的样式则会发生响应式修改
```
@media (max-width: 350px){
    :root{
        --fontcolor: green;
    }
}
```
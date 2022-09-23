---
title: "Vue3笔记"
date: 2021-12-24T14:57:21+08:00
---

# Vue 3.0  learning note

1. Life Cycle

与 2.0 的生命周期类似

2.0中生命周期是以选项的形式存在,而3.0中是以lifecycle hooks的形式存在，可以以`onX`的形式直接用`import`导入，而且，这些hooks只能在setup函数中同步执行
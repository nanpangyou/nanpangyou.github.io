---
layout: learnnote
title: homebrew节点
date: 2019-06-12 14:50:48
tags:
---

# 记录可用的 homebrew 节点，解决 brew update 卡死

每次使用`homebrew`的时候，会首先执行`brew update`。由于天朝网络的原因，update 几乎没有任何相应。只能先进行`ctrl+c`再执行安装命令.

解决办法

更改`homebrew`的镜像节点

```
# 进入brew主目录
$ cd `brew --repo`

# 更换镜像
$ git remote set-url origin https://git.coding.net/homebrew/homebrew.git

# 测试效果
$ brew update
```

可用的节点还有

- https://git.coding.net/homebrew/homebrew.git - Coding
- https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git - 清华
- https://mirrors.ustc.edu.cn/brew.git - 中科大

---
title: 使用npm-link或yarn-link
date: 2019-12-06 12:27:09
---

# 使用 npm link / yarn link

开发 npm 包的时候，如果想测试用户在使用自己的代码时会出现的问题，就需要先将代码通过`npm publish`发布到 npm 中，然后进行 npm install 或者 yarn add 来安装

一旦代码有问题，则需要重新指定版本号，重新发布，重新下载才能进行调试

遇到这种问题可以尝试在本地使用`npm link`或者`yarn link`来进行解决

用法大致如下：
当本地开发完代码后，可以在代码的目录里使用`npm link`或`yarn link`，然后去安装该包的目录里也相应使用`npm link <packageName>`或者`yarn link <packageName>`，这样安装目录里的包就会映射到本地的开发代码，而不是 npm 仓库下载的目录。

这样就可以你开发后直接看到用户使用你的包的效果了

如果想取消这种映射关系，需要使用`npm unlink <packageName>`或者`yarn unlink <packageName>`在两个目录里都执行一次。再重新安装一遍所依赖的包就可以了。

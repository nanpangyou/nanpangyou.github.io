---
title: 覆盖antdv的默认样式
date: 2020-12-06 23:49:23
---

在使用`ant-design-vue`时。

当默认样式不满足需求的时候，需要覆盖默认的样式，又不希望影响全局

1. 在`vue.config.js`中配置

```
module.exports = {
    css: {
        loaderOptions: {
            // 向 CSS 相关的 loader 传递选项 
            less: {
                lessOptions: {
                    javascriptEnabled: true
                }
            }
        }

    }
}
```

2. 在组件中。如果是less。则只需要在需要的选择器下添加对应的样式
如：
```
.filter-option {
    /deep/ .ant-radio-button-wrapper {
      z-index: 1;
      color: #4a50d9;
    }
    /deep/.ant-radio-button-wrapper-checked:not(.ant-radio-button-wrapper-disabled) {
      background: #4a50d9;
      color: rgba(255, 255, 255, 0.8);
    }
    /deep/.ant-radio-button-wrapper:not(:first-child)::before {
      background: transparent;
    }
```
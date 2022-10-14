---
title: "Vue3 Tsx 覆盖element Plus默认样式"
date: 2022-10-14T16:42:27+08:00
---

# Vue3 Tsx 覆盖element Plus默认样式

在使用tsx编写Vue3代码的时候遇到一个问题： 使用Tsx就没办法包含style标签，无法使用`::v-deep()`的方式来覆盖element Plus的默认样式。

## 解决方案

1. 全局覆盖
   只需要在main.ts中引入css文件即可（css文件中对想修改的样式重新编写）

2. 局部覆盖
   采用css-module的方式，将css文件命名为`xxx.module.css`，在tsx文件中引入,注意在对应的element样式前添加`:global`，如下：

```tsx
import s from "./xxx.module.scss";
export const xxx = defineComponent({
  setup: () => {
	return ()=>(
	<div class={s.wrapper}>
		<el-input class={s.xxx}></el-input>
	</div>
	)
  }
})
xxx.displayName = "xxx";
export default xxx;
```

```scss
// xxx.module.scss
.wrapper {
  :global(.el-input__inner) {
	color: red;
  }
}

```
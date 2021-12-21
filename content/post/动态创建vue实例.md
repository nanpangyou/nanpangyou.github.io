---
title: 动态创建vue实例

date: 2020-09-21 15:08:57
---
# 动态创建一个vue实例

在写`toast组件`时，需要在`toastPlugin`中使用`toast.vue`文件动态创建一个vue实例，将`toast组件`以及相关的样式加载进来（当然也可以使用`document.createElement`来做，但是原生dom无法使用vue组件的生命周期）

此时则需要在pulgin中动态创建一个vue实例

步骤：

1. 引入`toast.vue`文件
2. 使用`Vue.extend`创建构造函数
3. `new`一个vue实例
4. 设置相关的属性（比如slot，事件监听等）
5. `mount`到内存中
6. `append`到页面中

代码示例
```
import Toast from "./toast";
export default {
  install(Vue, options) {
    Vue.prototype.$toast = function(message,props) {
      let ToastConstructor = Vue.extend(Toast);
      let toast = new ToastConstructor({
        propsData: {...props}
      });
      toast.$slots.default = [message];
      toast.$mount();
      document.body.appendChild(toast.$el);
    };
  }
};


```
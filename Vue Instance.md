# Vue Instance

Vue实例用new Vue创建，它充当了MVVM模式中的ViewModel。创建Vue实例时需要传入一个Option对象，该对象可以包含数据，方法，组件生命周期hook等。

在选项对象中，通过el属性绑定View，通过data属性绑定Model

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'hello vue'
    }
});

var vm = new Vue({...}).$mount('#app')
```

## 实例的生命周期

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。

不要在选项 property 或回调上使用[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，因为箭头函数并没有 `this`，`this` 会作为变量一直向上级词法作用域查找，直至找到为止，经常导致 `Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function` 之类的错误。

![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)

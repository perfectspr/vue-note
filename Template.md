# Directive

指令 (Directives) 是带有 `v-` 前缀的特殊 attribute。指令 attribute 的值预期是**单个 JavaScript 表达式**.

指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

## 参数

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。

```html
<a v-bind:href="url">...</a>
```

## 动态参数

从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数：

动态参数预期会求出一个字符串，异常情况下值为 `null`。这个特殊的 `null` 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。

```html
<a v-bind:[attributeName]="url"> ... </a>
```

在 DOM 中使用模板时 (直接在一个 HTML 文件里撰写模板)，还需要避免使用大写字符来命名键名，因为浏览器会把 attribute 名全部强制转为小写：

```html
<!--
在 DOM 中使用模板时这段代码会被转换为 `v-bind:[someattr]`。
除非在实例中有一个名为“someattr”的 property，否则代码不会工作。
-->
<a v-bind:[someAttr]="value"> ... </a>
```

## 缩写

Vue 为 `v-bind` 和 `v-on` 这两个最常用的指令，提供了特定简写：

### v-bind缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

### v-on缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

## 内置指令

### v-show

v-show通过CSS属性display来控制元素的显示或隐藏

### v-if/v-else-if/v-else

控制Dom元素的创建与否

### v-for

#### 数组更新检测

以下数组的mutation method会被Vue检测到

* push

* pop

* shift

* unshift

* splice

* sort

* reverse

以下方法不能被监测到，因为它们返回一个新数组。可以通过替换旧数组的方式更新元素

* filter

* concat

* slice

Vue在检测到数组变化时，并不是直接重新渲染整个列表，而是复用DOM元素。含有相同元素的项不会被重新渲染，因此可以大胆使用新数组替换旧数组，而不需要担心性能问题。

以下方法引起的数组变动，Vue无法监测到：

```javascript
vm.books[0] = {title: 'vue'}; // 通过索引直接设置数组项
vm.books.length = 1; //修改数组长度, 用splice(1)解决
```

可以用以下方法解决：

```javascript
Vue.set(vm.books, 0, {title: 'vue'}) // 全局set方法
vm.$set(vm.books, 0, {title: 'vue'}) // 实例$set方法
vm.books.splice(1); // OK
```

#### 过滤于排序

通过计算属性来返回过滤或排序后的数组，而不用改变原始数据

```javascript
<li v-for="n in evenNumbers">{{n}}</li>

data: {
    numbers: [1, 2, 3, 4]
},
computed: {
    evenNumbers: function() {
        return this.numbers.filter(n=>n%2===0)
    }
}
```

#### 对象更新检测

Vue不能检测对象属性的添加和删除，只能通过方法调用更新

```javascript
Vue.set(vm.book, 'publishDate', '2010-01-01')
vm.$set(vm.book, 'publishDate', '2010-01-01')
Vue.delete(vm.book, 'isbn')
vm.$delete(vm.book, 'isbn')
```

### v-bind

```html
<img v-bind:src="imgSrc">
<img :src="imgSrc"> <!-- 缩写 -->
<img v-bind:[attrname]="imgSrc"> <!-- 动态属性 -->
<img :src="'images/' + fileName"> <!-- 内联字符串拼接 -->
```

### v-on

v-on指令的表达式可以是一段JS代码，一个方法名活着方法调用语句。

当Vue实例销毁时，所有事件处理器都会被自动删除。如果需要访问原始DOM事件，可以使用特殊变量$event 把它传入。也可以使用.prevent事件修饰符实现

```javascript
<a href="/login" @click="login($event)">Login</a>
//...
methods: {
    login(event) {
        event.preventDefault(); // 阻止跳转
    }
}
```

#### 事件修饰符

DOM事件模型。任何DOM事件首先进入捕获阶段，直到到达目标对象，然后进入冒泡阶段。

![Document Object Model Events](https://www.w3.org/TR/2003/NOTE-DOM-Level-3-Events-20031107/images/eventflow.png)

在事件处理程序中调用event.preventDefault()或者event.stopPropagation()非常常见，Vue提供事件修饰符简化这个问题。

修饰符可以串联使用，但顺序很重要。例如，用v-on:click.prevent.self会阻止所有的单击，而v-on:click.self.prevent只会阻止对元素自身的单击。

```html
<!-- 阻止单击事件继续传播 -->
<a @click.stop="doThis"></a>
<!-- 提交表单事件不再重新加载页面 -->
<form @submit.prevent="onSubmit"></form>
<!-- 只有当在event.target是当前元素自身时触发处理函数 -->
<div @click.self="doThat"></div>
<!-- 单击事件处理函数只执行一次 -->
<a @click.once="doThis"></a>
```

#### 按键修饰符

```html
<!-- 回车submit -->
<input @keyup.enter="submit">
<!-- 回车按键码submit -->
<input @keyup.13="submit">
<!-- Alt + C -->
<input @keyup.alt.67="clear">
<!-- Ctrl + Click-->
<input @click.ctrl="clear">
<!-- 即使同时按下Alt或者Shift键，也会触发 -->
<button @click.ctrl="onClick">A</button>
<!-- 只有按住Ctrl键，不按其他修饰键时才会触发 -->
<button @click.ctrl.exact="onClick">A</button>
<!-- 只有在没有按下系统修饰键时才会触发 -->
<button @click.ctrl="onClick">A</button>
<!-- 鼠标右键单击时才会触发 -->
<button @click.right="onClick">A</button>
```

### v-model

v-model本质上是语法糖，它负责监听用户的输入事件以更新数据。

### v-html

更新元素的innerHTML，容易导致XSS攻击，只在可信内容上使用v-html，永远不要在用户提交的内容上使用v-html

### v-once

指渲染一次当前元素或组件，提高性能

### v-text

### v-pre

跳过这个元素和其子元素的编译过程。

### v-cloak

### v-slot

## 自定义指令

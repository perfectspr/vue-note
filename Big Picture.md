# Big Picture

## MVVM

Model-View-ViewModel是一种简化用户界面的事件驱动编程方式。MVVM的核心是数据驱动， 即ViewModel，ViewModel是View和Model的关系映射，是一个值转换器（Value Converter），负责转换Model中的数据对象，使得数据变得更加易于管理和使用。

在MVVM中View和Model之间不能直接通信，ViewModel在中间充当观察者角色。

MVVM模式最核心的特性就是数据双向绑定，当用户操作View，ViewModel感知变化，然后通知Model。反之Model发生了变化，ViewModel感知并通知View进行更新。ViewModel向上与视图层进行双向数据绑定，向下与Model通过接口请求进行数据交互。

MVVM的核心理念是通过声明式的数据绑定来实现Model和View的分离。

![Android MVVM Design Pattern - JournalDev](https://cdn.journaldev.com/wp-content/uploads/2018/04/android-mvvm-pattern.png)

## Vue

Vue是基于MVVM模式的前端框架，它是以数据驱动和组件化的思想构建的。

ViewModel是Vue框架的核心，它是一个Vue实例。Vue实例作用于某个HTML上。

从View侧看，ViewModel中的DOM Listeners工具帮助我们监测页面上DOM元素的变化，如果有变化，则更改Model中的数据

从Model侧看，Model数据改变时，DataBindings工具（指令）帮助我们更新DOM元素。

前端开发中使用Vue框架，就是定义MVVM模式中的各个组成部分，用通过Vue提供的机制实现它们之间的交互。

![Getting Started - vue.js](https://012.vuejs.org/images/mvvm.png)

## Vue Ecosystem

![Thinking in components with Vue.js | by Shirish Nigam | Medium](https://miro.medium.com/max/5019/1*apBcw3f1BE8vJWAWG-k6gw.jpeg)

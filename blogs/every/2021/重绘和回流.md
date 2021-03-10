---
title: vue组件间的通信
date: 2021-02-27
tags:
 - vue
categories: 
 - vue
---

vue中组件间的通信是十分重要的，在这里记录一下常用的方法

最常见的几种类型如下

- 父组件向子组件传值：父组件绑定数据，通过`props`属性

- 子组件发布一个消息，父组件订阅，使用`$emit`方法

- 兄弟间通信，使用`$bus`总线，我一般的做法是在`main.js`中在Vue的原型上加一个bus属性，得到一个新的实例，之后若组件A向组件B传值，那么使用

  ```js
  this.$bus.$emit('event',this.msg)
  //event可以看作要传的事件标识名，this.msg就是需要传递的信息
  ```

  B接收A的值，使用

  ```js
  this.$bus.$on('event',res=>{console.log(res)})
  //event需要与发布者的事件标识相同
  ```

  bus属性除了在`main.js`中声明之外，还可以单独写在一个js文件中，需要的时候导入
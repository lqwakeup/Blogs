---
title: react hooks
date: 2021-06-05
tags:
 - react
categories:
 - react
---

react有两种方式声明组件，分别是函数式组件和类组件，函数式组件由于它的变量是局部的且没有生命周期钩子，所以没有得到广泛的应用，但是在hooks出现之后，总算解决了这个问题，并且弥补了类组件的许多缺陷。这里根据学的进度慢慢补充……

## useState

useState是一个hook，它的作用是用来保存在组件中使用的值

基本用法

```js
const [state, setstate] = useState(initialState)
```

useState返回一个存着两个值的数组，其中state 表示要存的值，setstate表示对state的修改，相当于setState({})


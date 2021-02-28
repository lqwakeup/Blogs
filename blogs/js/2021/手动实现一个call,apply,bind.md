---
title: 手动实现一个call,apply,bind
date: 2021-02-28
tags:
 - js
categories:
 - js
---

首先了解一下这三个函数，这三个函数都可以改变this的指向，主要区别是参数值和返回值的不同

| 函数  | 参数值                                          | 返回值                                                       |
| ----- | ----------------------------------------------- | ------------------------------------------------------------ |
| call  | `fn.call(thisArg,arg1,arg2...)`                 | 立即执行，返回值即函数返回值                                 |
| apply | `fn.apply(thisArg,[arg1,arg2...])`              | 立即执行，返回值即为函数返回值                               |
| bind  | `function.bind(thisArg[, arg1[, arg2[, ...]]])` | 返回一个绑定好第一个参数和后序参数的函数，需要接收然后再执行。 |



> 手写call 

call做的事情主要有两件

- 改变this的指向
- 调用函数，立即执行

```js
Function.prototype.myCall = function(context) {
    //首先判断调用它的是不是一个函数,不是的话，抛出一个类型错误
    if(typeof this !== "function"){
        throw new TypeError("this isn't function");
    }
    
    //如果没有上下文，那么默认指向window
    context = context || window;
    context.fn = this;
    
    //获取参数,由于第一个参数是需要作为this的对象，所以slice从1开始计算
    let args = Array.from(arguments).slice(1);
    //执行并拿到返回结果
    let result = context.fn(...args);
    
    //清除属性
    delete context.fn;
    
    return result;
}

//测试用例
let obj = {
    name: "ax"
}
function print(...args) {
    console.log(this);
    console.log(...args)
    console.log(this.name);
}

print.myCall(obj, "name", "mydear");
```

> 手写apply

apply和call的不同之处是
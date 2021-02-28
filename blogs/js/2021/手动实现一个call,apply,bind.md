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

apply和call的不同之处是参数的不同，它接收的第二个参数是一个数组

```js
Function.prototype.myApply = function(context) {
    if(typeof this !== "function"){
        throw new TypeError("this isn't function")
    }

    context = context || window;

    context.fn = this;
    let result;

    if(arguments[1]){
        result = context.fn(...arguments);
    }else{
        result = context.fn();
    }

    delete context.fn;
    return result
}

let obj = {
    name: "ax"
}
function print(...args) {
    console.log(this);
    console.log(...args)
    console.log(this.name);
}
```

> 手写bind

bind返回的是一个函数 而函数的调用有两种方式

第一种是直接调用另一种则是通过new调用

而在这两种方式中，如果使用new调用，那么当前的this指向的就是这个函数对象||函数返回对象， 对于这种情况 我们不需要再为其绑定this，因为this已经指向了其要指向的目标

```js
        // context :要bind的对象
        Function.prototype.myBind = function (context) {
            if (typeof this !== "function") {
                throw new TypeError("this is not a function");
            }
            // bind的返回值是一个函数 这个函数可以通过new调用，也可以直接调用
            // 提取bind的参数,因为第一个是context对象 所以需要分割
            const args = Array.from(arguments).slice(1);
            // 获取context,如果没有就指向为window
            context = context || window;
            // 用_this变量保存当前的函数（在当前作用域this指向的就是函数体本身）
            const _this = this;
            return function F() {
            // 返回的函数中 如果this的原型链上有F，说明是通过new进行调用的，那么我们不需要为其绑定this
            // 否则就是直接调用，那么我们需要为其绑定this
                if (this instanceof F) {
                    return _this(...args);
                } else {
                    return _this.apply(context, args);
                }
            }
        }
 
        // 测试用例
        function print(name, age) {
            if (name && age) {
                this.name = name;
                this.age = age;
            }
            console.log(`hi,I am {this.name},I am{this.age} years old!`);
        }
        let obj = {
            name: "ax",
            age: 20
        }
        let obj2 = {
            name: "kime",
            age: 19
        }
        // 直接调用
        console.log( obj) //{name: "ax", age: 20}
        let f1 = print.myBind(obj);
        f1(); //hi,I am ax,I am 20 years old!
        // 直接调用参数改变
        f2 = print.myBind(obj, "AX", 21);
        // 执行f
        f2();//hi,I am AX,I am 21 years old!
        // 打印看obj是否也发生了变化:确实发生了变化
        console.log(obj);//{name: "AX", age: 21}
 
        //通过new调用
        let f3 = print.myBind(obj, "AX", 30);
        let funcObj = new f3();
        console.log(funcObj)
```




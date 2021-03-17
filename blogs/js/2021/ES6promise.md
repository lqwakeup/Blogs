---
title: ES6新特性——promise
date: 2021-03-16
tags:
 - js
categories:
 - js
---

ES6出了许多好用的新特性，其中的典型代表就是接下来要出场的这位了——promise！

众所周知，JavaScript是一门单线程的语言，只能从上到下一个个执行。这样的执行流程虽然对开发人员非常友好，但是对用户却不那么友好。比如说我定义一个定时器，如果按顺序执行，那么下一个操作必须要等待定时器执行完成。这样是非常慢的！因此，大大们引入了promise这个概念。

`Promise`对象有以下两个特点。

（1）对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。



下面通过手动实现一个简单的promise来弄清楚它的工作流程。

```js
// promise有三种状态
const PENDING = "pending"
const FULFILLED = "fulfilled"
const REJECTED = "rejected"

function myPromise(callback) {
    var _this = this;
    // 初始状态
    _this.currentState = PENDING;
    _this.value = 0;

    _this.onResolveCallbacks = [];
    _this.onRejectedCallbacks = [];

    _this.resolve = function(value) {
        if(value instanceof myPromise){
            return value.then(_this.resolve,_this.rejected);
        }
        setTimeout(()=>{
            if(_this.currentState == PENDING){
                _this.currentState = FULFILLED;
                _this.value = value;
                _this.onResolveCallbacks.forEach(cb=>cb())
            }
        })
    }

    _this.rejected = function(error) {
        setTimeout(()=>{
            if(_this.currentState == PENDING){
                _this.currentState = REJECTED;
                _this.value = this.value;
                _this.onRejectedCallbacks.forEach(cb=>cb())
            }
        })
    }

    try {
        callback(_this.resolve,_this.rejected)
    }catch(e){
        _this.rejected(e)
    }
}

myPromise.prototype.then = function(){}
```

`Promise`为我们提供了一些方法，方便用户使用。

`Promise`原型方法：

- Promise.prototype.catch(onRejected)
- Promise.prototype.finally(onFinally)

`Promise`自身的方法：

- Promise.resolve(value)
- Promise.reject(reason)
- Promise.all(iterable)
- Promise.race(iterable)
- Promise.allSettled(iterable)

下面写一些比较重要的方法



## Promise.all()

首先明确一下 `Promise.all()` API 的功能，其在 MDN 中的介绍如下：

> `Promise.all(iterable)` 方法返回一个 `Promise` 实例，此实例在 `iterable` 参数内所有的 `promise` 都“完成（resolved）”或参数中不包含 `promise` 时回调完成（resolve）；如果参数中 `promise` 有一个失败（rejected），此实例回调失败（reject），失败原因的是第一个失败 `promise` 的结果。

```js
Promise.myAll = function(promiseArr) {
    let results = [];
    return new Promise((resolve,reject)=>{
        let i=0;n=0;
        while(n<promiseArr.length){
            promiseArr[n].then(res=>{
                results[n] = res;
                i++;
                if(i === promiseArr.length){
                    resolve(results);
                }
            }).catch(err=>{
                reject(err);
            });
            n++;
        }
    })
}
```



## Promise.race()

`Promise.race()` 在 MDN 中给出的定义为：

> `Promise.race(iterable)` 方法返回一个 `promise`，一旦迭代器中的某个 `promise` 解决或拒绝，返回的 `promise` 就会解决或拒绝。

```js
Promise.myRace = function(promiseArr) {
    return new Promise((resolve,reject)=>{
        let n=0;
        while(n<promiseArr.length) {
            promiseArr[n].then(res=>{
                resolve(res);
            }).catch(err=>{
                reject(err);
            })
            n++;
        }
    })
}
```



## Promise.allSettled()

在之前了解 `Promise.all()` 的用法和实现过程中，可以发现 `Promise.all()` 其实存在一个问题：只要所有的 Promise 对象中有一个被 reject，那么其他已经 resolve 的 Promise 对象们会被忽略。因此可以使用 `Promise.allSettled()` 方法来解决这一问题。`Promise.allSettled()` 方法在 MDN 中的定义如下：

> `Promise.allSettled()` 方法返回一个在所有给定的 `promise` 已被决议或被拒绝后决议的 `promise`，并带有一个对象数组，每个对象表示对应的`promise` 结果。

根据定义，我们的思路如下： 1. 首先同样运行所有的 Promise 对象； 2. 与 `Promise.all` 不同的是，在有 Promise 被 reject 之后，我们不会直接 reject，而是记录下该 reject 的值和对应的状态 'rejected'；同样地，当 Promise 对象被 resolve 时我们也不仅仅局限于记录值，同时也会记录状态 'fulfilled'。当所有的 Promise 对象都已决（解决或拒绝）后，我们统一 resolve 记录了值（value）已决状态（status）的数组。

```js
Promise.newAllSettled = function (promiseArr) {
  let results = [];
  return new Promise((resolve, reject) => {
    let i = 0, n = 0;
    // 运行所有的 Promise
    while (n < promiseArr.length) {
      promiseArr[n].then(res => {
        // 当有 Promise 被 resolve 之后，记录 resolve 值和状态，已决 Promise 计数加一
        results.push({value: res, status: 'fulfilled'});
        i++;
        // 全部 Promise 已决，resolve
        if (i === promiseArr.length) {
          resolve(results);
        }
      }).catch(err => {
        // 当有 Promise 被 reject 后，记录 reject 值和状态，并且已决的 Promise 计数加一
        results.push({value: err, status: 'rejected'});
        i++;
        if (i === promiseArr.length) {
          resolve(results);
        }
      });
      n++;
    }
  })
};
```

还有一些其他的方法。下面这位大大有写
[promise所有方法的实现](https://zhuanlan.zhihu.com/p/107712626)
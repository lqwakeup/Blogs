---
title: 使用getter，setter控制属性访问
date: 2021-03-02
tags:
 - js
categories:
 - js
---

在JavaScript中，对象是相对简单的属性集合。保持程序状态的主要方法是修改对象的这些属性。js为我们提供了get，set两个关键字来实现getter，setter方法，很好的为我们完成了上述需求。

之前学习vue，发布订阅者模式这个词的出现频率已经让我记忆深刻，他的实现其实也借助了getter，setter。它的实现其实就两个步骤：

**在getter中收集依赖，在setter中触发依赖**

通过`Object.defineProperty`这个方法完成侦测，下面简单写一下他的用法

```js
/**
 * 直接访问对象，构造一个fullName构造器
 */
let vm = {
    name:'xiaoqiao',
    lastName:'haha'
}

Object.defineProperty(vm,'fullName',{
    get() {
        console.log('getter');
        return this.name+this.lastName;
    },
    set(value) {
        console.log('setter')
        this.name = value;
    }
})

console.log(vm.fullName);//隐式调用getter
console.log(vm.fullName = "justtest")//隐式调用setter

/**
 * 使用构造函数，闭包访问属性
 */

function Person() {
    let _n1 = 'hello js'

    Object.defineProperty(this,'n1',{
        get() {
            console.log('fn getter');
            return _n1;
        },

        set(newN1) {
            console.log('fn setter');
            _n1 = newN1;
        }
    })
}

let person = new Person();
console.log(person.n1);
console.log(person.n1 = 'hello vue');
console.log(person.n1);
```






---
title: 普通函数this指向
date: 2021-04-28
tags:
 - js
categories:
 - js
---

普通函数this指向详解

this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象（这句话不那么严谨，作为一般情况下是可以的）

section1：如果一个函数中有this，但是它没有被上一级的对象所调用，那么this指向的就是window，这里需要说明的是在js的严格版中this指向的不是window，但是我们这里不探讨严格版的问题(在严格版中的默认的this不再是window，而是undefined。)

```js
function dog(){
    var user = "二狗子";
    console.log(this.user); //undefined
    console.log(this); //Window
}
dog(); //相当于window.a();
//this最终指向的是调用它的对象，这里的函数a实际是被Window对象所点出来的
```

 

section2：如果一个函数中有this，这个函数有被上一级的对象所调用，那么this指向的就是上一级的对象。

```js
var dog = {
    user:"二狗子",
    fn:function(){
        console.log(this.user); //二狗子
        console.log(this); //{user: "二狗子", fn: ƒ}
    }
}
dog.fn(); //this执行时被它的上一级对象dog{user: "二狗子", fn: ƒ}调用
```

 

section3：如果一个函数中有this，这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它上一级的对象

```js
var dog = {
    a:1,
    b:{
        //a:2,
        fn:function(){
            console.log(this.a); //undefind  有两个对象b和dog,所以此this.a指向它的上一级
        }
    },
    fn1:function(){
        console.log(this.a);  //1 
    }
}
dog.fn1();
dog.b.fn();
```

另外还有两种种特殊情况：

第一种：当this遇到return

1

```js
function fn()  
{  
    this.user = '二狗子';  
    return {};  
}
var a = new fn;  
console.log(a.user); //undefined
```

 2.

```js
function fn()  
{  
    this.user = '二狗子';  
    return function(){};
}
var a = new fn;  
console.log(a.user); //undefined3.
function fn()  
{  
    this.user = '二狗子';  
    return 1;
}
var a = new fn;  
console.log(a.user); //二狗子4.
function fn()  
{  
    this.user = '二狗子';  
    return undefined;
}
var a = new fn;  
console.log(a.user); //二狗子
```

 

```js
function fn()  
{  
    this.user = '二狗子';  
    return null;
}
var a = new fn;  
console.log(a.user); //二狗子
```

总结：如果返回值是一个对象，那么this指向的就是那个返回的对象，如果返回值不是一个对象那么this还是指向函数的实例。
还有一点就是虽然null也是对象，但是在这里this还是指向那个函数的实例，因为null比较特殊。

 


第二种：

```js
var dog = {
    a:1,
    b:{
        a:2,
        fn:function(){
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn;
j();
```

 

这里this指向的是window，是不是有些蒙了？其实是因为你没有理解一句话，这句话同样至关重要。

　　this永远指向的是最后调用它的对象，也就是看它执行的时候是谁调用的，例子4中虽然函数fn是被对象b所引用，但是在将fn赋值给变量j的时候并没有执行所以最终指向的是window


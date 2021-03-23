---
title: js题目
date: 2021-03-19
tags:
 - every
categories: 
 - every
---

js的面试题总是考的非常综合，在这里慢慢总结，能加到多少是多少。

```js
var a = 0,b = 0;
function A(a) {
	A = function(b) {
		console.log(a+b++);
	};
	console.log(a++);
}

A(1)
A(2)
```

解答：结果是1和4

解析：首先第一个A(1)，是调用外面的function A，这时候函数A有个形参a，这就相当于在函数作用域内写了这样一句话 var a = 1。根据作用域和作用域链规则，此时的a取1，然后再函数A里面对函数进行了重新定义，之后输出a++，先输出再执行。所以A(1)输入结果1。

在A(2)中，A已经被重新定义，相当于此时执行的是里面的函数，所以此时形参b = 2。由于这个函数里面用了参数a，沿着作用域链向上查找，发现在外层函数中用a，所以外面函数没有销毁，延长了声明周期。此时啊a=2,b=2。输出4。



```js
var i = 0;
for(var i = 0;i<5;i++){
	setTimeout(function timer(i){
		console.log(i)
	},i*100)
}
```

解答：输出5个undefined。

在定时器中有定义了一个函数timer，其形参为i。相当于在timer函数体中加入了一句话：var i；沿着作用域链，在本函数体中找到了i之后，就不会继续向上查找，所以输出undefined。



```js
var a = 1;
function foo() {
	a = 10;
	console.log(a);
	return;
}
foo();
console.log(a);// 10 10
--------------------------------
var a = 1;
function foo() {
	var a = 10;
	console.log(a);
	return;
}
foo();
console.log(a);// 10 1
--------------------------------
var a = 1;
function foo() {
	a = 10;
	console.log(a);
	return;
    function a() {};
}
foo();
console.log(a);// 10 1

```

解答：输出结果如上图

首先，第一个，在函数外var a = 1，在函数里面a直接赋值10，那么的话在这里改变a,就相当于改变了全局的a，所以这时候结果是10，10。

第二个，a是函数作用域里面的值，函数执行完了里面的a被销毁，所以此时结果为10，1。

第三个，在foo函数里面a函数声明提升，此时相当于a在foo作用域里面，这时候将变量a赋值为10，那这时候a也不会改变外面的a，所以，结果打印为10 1。



```js
console.log(bar);         // f bar() { console.log(123) }
console.log(bar());       // undefined
var bar = 456;
function bar() {
    console.log(123);     // 123
}
console.log(bar);         // 456
bar = 789;
console.log(bar);         // 789
console.log(bar())        // bar is not a function
```

解答：输入结果如上图

这题需要注意的一点：**函数声明的提升优先于变量提升！！！**所以在这里function bar() 被提到了最上面。

所以第一行打印函数。

由于调用函数bar没有返回值，所以直接console.log(bar())输出的是undefined。但是在函数体内有一句打印123，在调用后，这一句会调用123。

之后bar又被变量重新赋值，故后面打印456，789。

此时bar已经不是函数，不能被调用，因此最后一句会抛出一个错误，表明bar不是一个函数。



```js
var a = [];
for(let i=0;i<10;i++){
	a[i] = function() {
		console.log(i);
	}
}

a[6]();//6

-------------------------------

var a = [];
for(var i=0;i<10;i++){
	a[i] = function() {
		console.log(i);
	}
}

a[6]();//10
```

解答：输入结果如上图

这题考查的内容：let和var的作用域。第一个，for循环中使用let关键字，let有块级作用域，所以每个i只在当前循环有效，每次循环的i其实都是一个新的变量，于是最后输出的是6。第二题，for循环中使用var关键字，i是全局的，循环中相当于使用了一个全局变量i，所以到最后，输出的值都是最终的i值，结果为10。



```JS
for(let i=0;i<3;i++){
	var i = 'abc'
	console.log(i)
}
// SyntaxError: Identifier 'i' has already been declared


for(var i=0;i<3;i++){
	var i = 'abc'
	console.log(i)
}
// abc

for(var i=0;i<3;i++){
	let i = 'abc'
	console.log(i)
}
// abc
// abc
// abc

for(let i=0;i<3;i++){
	let i = 'abc'
	console.log(i)
}
// abc
// abc
// abc
```

解答：输入结果如上图

考查作用域。








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










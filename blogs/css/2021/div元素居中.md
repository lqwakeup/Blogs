---
title: div水平居中
date: 2021-04-28
tags:
 - css
categories: 
 - css
---

让一个div元素水平居中有许多方式，下面重点说两种我不怎么熟悉的。

> margin : auto

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		
		<style type="text/css">
			#as {
				/*text-align: center;
				display: flex;
				justify-content: center;
				text-align: center; */
			}
			#app{
				
				width: 100px;
				height: 100px;
				margin: auto;
				background-color: #FF0000;
			}
		</style>
	</head>
	<body>
		<div id="as">
			<div id="app">
				<span>adsad</span>
			</div>
		</div>
	</body>
</html>

```

> display:inline-block

父元素`text-align`属性设置为`center`，再将需要居中显示的div元素设置的dispaly设置为inline-block，使其具有内联元素的特性，这样，父元素的text-align也会对它产生影响。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		
		<style type="text/css">
			#as {
				text-align: center;
				/* display: flex;
				justify-content: center;
				text-align: center; */
			}
			#app{
				display: inline-block;
				width: 100px;
				height: 100px;
				/* margin: auto; */
				background-color: #FF0000;
			}
		</style>
	</head>
	<body>
		<div id="as">
			<div id="app">
				<span>adsad</span>
			</div>
		</div>
	</body>
</html>
```








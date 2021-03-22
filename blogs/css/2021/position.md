---
title: position
date: 2021-03-22
tags:
 - css
categories: 
 - css
---

讲position的文章有许多，我看了一下，觉得菜鸟教程讲的就很好！

以下内容引用自菜鸟教程[原文地址](https://www.runoob.com/css/css-positioning.html)

position 属性指定了元素的定位类型。

position 属性的五个值：

- [static](https://www.runoob.com/css/css-positioning.html#position-static)
- [relative](https://www.runoob.com/css/css-positioning.html#position-relative)
- [fixed](https://www.runoob.com/css/css-positioning.html#position-fixed)
- [absolute](https://www.runoob.com/css/css-positioning.html#position-absolute)
- [sticky](https://www.runoob.com/css/css-positioning.html#position-sticky)

元素可以使用的顶部，底部，左侧和右侧属性定位。然而，这些属性无法工作，除非是先设定position属性。他们也有不同的工作方式，这取决于定位方法。

## static 定位

HTML 元素的默认值，即没有定位，遵循正常的文档流对象。

静态定位的元素不会受到 top, bottom, left, right影响。

## 实例

div.static {    position: static;    border: 3px solid #73AD21; }


[尝试一下 »](https://www.runoob.com/try/try.php?filename=trycss_position_static)

------

## fixed 定位

元素的位置相对于浏览器窗口是固定位置。

即使窗口是滚动的它也不会移动：

## 实例

p.pos_fixed {    position:fixed;    top:30px;    right:5px; }


[尝试一下 »](https://www.runoob.com/try/try.php?filename=trycss_position_fixed)

**注意：** Fixed 定位在 IE7 和 IE8 下需要描述 !DOCTYPE 才能支持。

Fixed定位使元素的位置与文档流无关，因此不占据空间。

Fixed定位的元素和其他元素重叠。

------

## relative 定位

相对定位元素的定位是相对其正常位置。

## 实例

h2.pos_left {    position:relative;    left:-20px; } h2.pos_right {    position:relative;    left:20px; }


[尝试一下 »](https://www.runoob.com/try/try.php?filename=trycss_position_relative)

移动相对定位元素，但它原本所占的空间不会改变。

## 实例

h2.pos_top {    position:relative;    top:-50px; }


[尝试一下 »](https://www.runoob.com/try/try.php?filename=trycss_position_relative2)

相对定位元素经常被用来作为绝对定位元素的容器块。

------

## absolute 定位

绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于<html>:

## 实例

h2 {    position:absolute;    left:100px;    top:150px; }


[尝试一下 »](https://www.runoob.com/try/try.php?filename=trycss_position_absolute)

absolute 定位使元素的位置与文档流无关，因此不占据空间。

absolute 定位的元素和其他元素重叠。

------

## sticky 定位

sticky 英文字面意思是粘，粘贴，所以可以把它称之为粘性定位。

**position: sticky;** 基于用户的滚动位置来定位。

粘性定位的元素是依赖于用户的滚动，在 **position:relative** 与 **position:fixed** 定位之间切换。

它的行为就像 **position:relative;** 而当页面滚动超出目标区域时，它的表现就像 **position:fixed;**，它会固定在目标位置。

元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。

这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

**注意:** Internet Explorer, Edge 15 及更早 IE 版本不支持 sticky 定位。 Safari 需要使用 -webkit- prefix (查看以下实例)。

## 实例

div.sticky {    position: -webkit-sticky; /* Safari */    position: sticky;    top: 0;    background-color: green;    border: 2px solid #4CAF50; }


[尝试一下 »](https://www.runoob.com/try/try.php?filename=trycss_position_sticky)

------

## 重叠的元素

元素的定位与文档流无关，所以它们可以覆盖页面上的其它元素

z-index属性指定了一个元素的堆叠顺序（哪个元素应该放在前面，或后面）

一个元素可以有正数或负数的堆叠顺序：

## 实例

img {    position:absolute;    left:0px;    top:0px;    z-index:-1; }


[尝试一下 »](https://www.runoob.com/try/try.php?filename=trycss_zindex)

具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面。

**注意：** 如果两个定位元素重叠，没有指定z - index，最后定位在HTML代码中的元素将被显示在最前面。
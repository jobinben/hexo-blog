---
title: JavaScript——类型检测
date: 2019-02-11 16:40:04
tags:
---

## typeof
**typeof** 检测运算是最常见的，适合函数对象和基本类型判断。

> typeof 100     返回 "number"
> typeof true     返回 "boolean"
> typeof function  返回 "function"
<!--more-->
> typeof (undefined) 返回 "undefined"
> typeof new Object() 返回 "object"
> typeof [1,2] 返回 "object"
> typeof NaN 返回 "number"
> typeof null 返回 "object"

这里要注意为什么null返回会是object呢？**typeof null === "object" ?**
是因为一些历史的原因，把null改为返回null的情况下，大部分网站无法访问到，也可以说是兼容性的问题。

## instanceof
**instanceof** 适合函数对象或者函数构造器判断。

> [1,2] instanceof Array === true
> new Object() instanceof Array === false

**Caution !** 不同window或iframe间的对象类型检测不能使用instanceof。


## Object.prototype.toString
判断一些数组，函数也可以用Object.prototype.toString。

> Object.prototype.toString.apply([]) === "[object Array]"
> Object.prototype.toString.apply(function(){}) === "[object Function]"
> Object.prototype.toString.apply(null) === "[object Null]"
> Object.prototype.toString.apply(undefined) === "[object Undefined]"

**Caution !** 在IE6/7/8 Object.prototype.toString.apply(null) 返回 "[object Object]"

## 类型检测小结
**typeof** 适合基本类型及function检测，遇到null失效。

**[[Class]]**  通过{}.toString拿到，适合内置对象和基本类型，遇到null和undefined失效（IE678等返回[object Object]）。

**instanceof** 适合自定义对象，也可以用来检测原生对象，在不同iframe和window间检测时失效。

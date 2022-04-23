---
title: 关于js的面向对象（OOP）上
date: 2019-03-02 21:54:50
tags:
---
面向对象的特征 主要四部分如下：
 1. 继承
 2. 封装
 3. 多态
 4. 抽象

## 继承
继承是实现复用性的一个重要的手段。
<!--more-->
 **1. 基于原型的继承**
 当我们定义一个函数，函数都有一个内置的对象，叫**prototype** 
可以通过prototype来继承一些属性，例如：

> function  Foo(){
> this.y= 2 ;
> }
> typeof Foo.prototype;  输出 "object";
> 给Foo原型定义一个属性
> Foo.prototype.x = 1;
> 然后实例一下对象Foo（）；
> var obj = new Foo();
> 我们来调用obj的原型；
> obj.y; 输出//2
> obj.x;输出//1;

上面的obj就是继承了Foo.prototype原型中的x；也继承了obj实例的对象里的y。
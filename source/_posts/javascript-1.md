---
title: JavaScript——类型（一）
date: 2019-02-04 23:08:57
tags:
---

在学习类型之前，我们先来看一下基础的类型运算。
**var num=32； num=“This is String”；** 这样是不是觉得JavaScript好简单，带着疑问看下下面的题。
当32+32等于64，这样是简单的数字运算。
当 **“32”+32** 会等于多少呢？答案会是64。对的他们是字**符串拼接**！
如果是 **“32”-32** 呢？它会等于0。为什么呢？因为字符串已经被默认转化为数字类型 进行运算。
由此我们得出：**当字符串与数字如果进行加法运算，那它们将进行字符串拼接。如果字符串与数字进行减法运算，就会把字符串转化为int类型，praseInt（‘32’）=32，然后进行运算。（如果字符串不能转化成数字，那么结果将会是NaN,NaN与任何数运算都会等于NaN）**
<!--more-->
**javascript有两大种类型：**

## 原始类型
 - Number
 - String
 - Null
 -  boolean
 -  undefined

## 其他类型

 - Object 也叫对象类型。那么JavaScript的函数、数组、日期等也是对象类型。




    彩蛋： 巧用+/-规则转化类型，定义一个变量num，当你想要一个数字类型 **（num-0）** num减去一个零；
    	  想要一个字符串 **（num+" "）** num加上一个空字符串即可。

<div align="center">[祝大家猪年快乐！诸事顺利](https://blog.csdn.net/qq_41308489/article/details/86764563)</div>
<img src="javascript-1/newYear.jpg" height=50% width=50%>

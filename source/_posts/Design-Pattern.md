---
title: 设计模式——单例模式
date: 2019-03-03 23:19:14
tags:
---
首先我们先了解一下设计模式是什么呢？
**设计模式（Design Pattern）：是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。**
 
 使用设计模式的目的：使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。

首先我们先来认识一下最简单的**单例模式**！**单例模式**是什么呢？

单例模式：我们可以把它看做一个皇帝，我们古代的皇帝有且一个，因为一山容不下二虎，如果有第二个就会导致很多问题。比如说我们当代的老婆一样，老婆只需要一个就可以了，如果多了一个就会出现复杂的问题。
有些对象我们只需要一个，比如：
**配置文件、工具类、线程池、缓存、日志对象**等。
如果创造出多个实例，就会导致许多问题，比如占用过多资源，不一致的结果等。 
<!--more-->
常用的单例模式有两种：

 1. 饿汉模式
 2. 懒汉模式

## 饿汉模式
饿汉模式的特点是加载类时比较慢，但运行时获取对象的速度比较快

> class Singleton(){ 	
> 	//1.将构造方法私有化，不允许外部直接创建对象 	
> 	private Singleton(){
> 		} 	
> 	//2.创建类的唯一实例，使用private static修饰符 		
> private static Singleton instance = new Singleton(); 		
> //3.提供一个用于获取实例的方法 使用public static修饰符才可以在其他地方调用 		
> public static Singleton getInstance(){
> 		   	return instance;
> 		   	} 	} 
> 调用方法
>  var s1 = Singleton.getInstance(); 
>  var s2 = Singleton.getInstance(); 
>  console.log(s1==s2); 输出为true，是同一个实例
>   如果用new Singleton（）；这是就会报错，因为只能有一个实例。

## 懒汉模式
懒汉模式的特点是加载类时比较快，但运行时获取对象的速度比较慢

> class Singleton2{
>  		//1.将构造方法私有化，不允许外部直接创建对象 
>  	private Singleton2(){}
>  //2.声明类的唯一实例，使用private static修饰
>  private static Singleton2 instance;
>  //3..提供一个用于获取实例的方法 使用public static修饰符
>  public static Singleton2 getInstance(){
>  if(instance == null){
>  	instance = new Singleton2();
>  }
>  return instance;
> }
>  
>  调用懒汉模式
>  var s3= Singleton.getInstance(); 
>  var s4 = Singleton.getInstance(); 
>  console.log(s3==s4); 输出为true，是同一个实例
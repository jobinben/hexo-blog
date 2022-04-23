---
title: 关于TypeScript的面向对象（OOP）
date: 2019-03-03 21:50:18
tags:
---
面向对象是什么呢？在维基百科里面说道：
**面向对象程序设计（Object-oriented programming , OOP）是一种程序设计范型，同时也是一种程序开发的方法。对象指的是类的实例。它将对象作为程序的基本单元，将程序和数据封装其中，以提高软件的重用性、灵活性和扩展性**

关于TypeScript的面向对象主要特性有以下这些：

 1. 类
 2. 类的继承
 3. 范型（generic）
 4. 接口（interface）
 5. 模块（Module）
 6. 注解（annotation）
 7. 类型定义文件（*.d.ts）
<!--more-->
**类**
类里面有这么几个重要的修饰符：protected、private、public。我们用代码例子来一一说明。

> class Person{
> //构造函数，只在类被实例化时调用，且只被调用一次
> 	constructor(name : string){
>  	this.name = name;
>  }
>  //访问控制修饰符
>  private age; //只可以在类内部访问
>  protected sex; //只可以在类内部或子类的内部被访问
>  public name； //可以在类外被访问  如果一个变量没有给修饰符就默认为public
>  hi(){ //没有修饰符，默认为public
>   console.log(this.name)
>   }
>   }

定义的类我们可以多次调用它

> var  a= new Person("jobin") //因为构造函数中有参数，所以实例化的时候必须传入参数
> a.hi()；	//输出为 jobin
> var b = new Person("dalao") 
> b.hi()；	//输出dalao； 


## 类的继承
两个关键字：

 1. extends : 用来声明类的继承关系
 2. super ： 用来调用父类的构造函数或者方法
 
 **extends关键字**
 所谓继承关系是一种“是”的关系，如：“狗是动物”，“服务员是人”等。当一个类使用extends关键字继承一个父类后，那么这个类会获得它继承到父类的属性和方法。
 如我们继承以上定义的Person类
 

> class  Foo extends Person{
> //创建一个方法
> cool(){
> console.log("is cool");
> }
> }
> //接着我们来实例化后调用它
> var c = new Foo("john");
> c.hi(); //输出john
> c.cool(); //输出is cool

**super关键字**

> class  Foo extends Person{
> //创建一个方法
> cool(){
> super.hi() //在子类中通过super关键字调用父类的方法
> console.log("is cool");
> }
> }
> //接着我们来实例化后调用它
> var d = new Foo("john");
> d.cool(); //输出johnis cool

## 泛型(generic)
泛型是指一种参数化的类型，一般用来限制集合的内容
结合以上的代码

> 我们定义一个泛型
> var hicool ：Array＜Person＞ = [] ;//<>里面放哪个类就是哪个类的泛型。数组只能放Person的类型的数据
> hicool[0] = new Person("xiaoming") ; 
> hicoo[1] = new Foo("lisi");
> hicool[2] = 2;//2是一个number类型 不是Person的数据，这个时候就会报错。


## 接口（Interface）
接口是用来建立某种代码约定，使得其他开发者在调用某个方法或者创建新的类时必须遵循所接口定义的代码约定。

**Interface : 用来声明一个接口
Implements : 用来声明某个类实现了某一个接口**

接口的使用方式第一种：**用作函数参数的类型声明**

> interface  IPerson{
>  	name: sting;
>  	age:number;
>  }
class Person {
   constructor(public config: IPerson){ //接口用作函数参数的类型声明
   }
   }
   var aa = new Person({
   name : "小明",
   age: 100 ,
   //多传或少传参数都会报错
   })
   
接口的使用方式第二种：在接口中声明方法，则所有实现该接口的类都得实现这个方法。

>interface Animal{
>eat()
>}
>class  Sheep implements Animal{	//Sheep类实现了Animal接口
>//当一个类实现某个接口时，它必须实现该接口中声明的方法
>eat(){
>	console.log(" i eat grass")
>}
>}
>class Tiger implements Animal{ //Tiger 类实现了Animal接口
> eat(){
> console.log("i eat sheep")
> }
> }


## 模块(Module)
模块可以帮助开发者将代码分割为可重用的单元。开发者可以自己决定将模块中的哪些资源（类、方法、变量）暴露出去供外部使用，哪些资源只在模块内使用。
所谓“模块”在typescript中就是一个文件，一个文件就是一个模块。在模块内部，有两个关键字来支撑模块的特性；这两个关键字是“export（导出）”和“import（导入）”。
先创建一个a.ts

> export var prop1;//对外暴露的变量
> 
> var prop2;//不对外暴露的变量
> 
> export function func1() {//对外暴露的方法
>      }
> 
> function func2(){//不对外暴露的方法
> 
> }
> 
> export class Clazz1{//对外暴露的类
> 
> }
> 
> class Clazz2{//不对外暴露的类
>      }

在创建一个文件b.ts

> //引入a.ts中暴露出来的变量、方法、类 import {prop1,func1,clazz1} from './a'
> 
> console.log(prop1) func1(); new clazz1()
> 
> //也可以向外暴露自己的方法 export function func3() {
>      }

## 注解（annotation）
注解为程序的元素（类、方法、变量）加上更直观、更明了的说明，这些说明信息与程序的业务逻辑无关，而是供指定的工具或框架使用的。

## 类型定义文件（*.d.ts）
类型定义文件用来帮助开发者在ts中使用已用的js工具包。

---
title: TypeScript设计模式的工厂模式
date: 2019-03-10 00:34:38
tags:
---
## 简单工厂模式
**特点**：**就是把同类型产品对象的创建集中到一起，通过工厂来创建，添加新产品时只需加到工厂里即可，也就是把变化封装起来，同时还可以隐藏产品的一些细节。**

**用处**：**要new多个同一类型对象时可以考虑使用简单工厂。**

**注意**：**对象需要继承自同一个接口**

我们来举例一下枪工厂：

> `enum GunType{
> AK, M4}`
> `interface Shootable { 	shoot(); }`
> `abstract class Gun implements Shootable {
> //抽象产品 - 枪 
> `
<!--more-->
> 	`abstract 	shoot(); `
> `}`
> //创建两个类，一个AK47 、 一个M4A1 并且继
> shoot(){ console.log('ak47 shoot') }`承抽象类Gun
> `class AK47 extends Gun {
> `}`
> `class M4A1 extends Gun { shoot(){ console.log('m4a1 shoot') }`
> `}`
> //现在我们来创建一个简单工厂
> `class GunFactory { `
> `static createGun (type: GunType) : Gun {`
> `switch (type) {`
> `case GunType.AK:`
> `return new AK47();`
> `case GunType.M4:`
> `return new M4A1();`
> `default: throw Error(' 还不支持这只枪');`
> `}`
>  ` }`
> `}`
> //接下来我们来调用我们想要用的枪看看
> `GunFactory.createGun (GunType.AK).shoot();`
> `GunFactory.createGun (GunType.M4).shoot();`
> //输出
> ak47 shoot;
> m4a1 shoot;

上面代码GunFactory工厂就是根据类型来创建不同的产品，使用的时候只需要引入这个工厂和接口即可。比如加个AWM：
直接
`class AWM extends Gun { shoot(){ console.log('AWM shoot') } }` 
然后在工厂里面上返回一个`new AWM()`。


## 工厂方法模式
特点：把工厂抽象出来，让子工厂来决定怎么生产产品，每个产品都由自己的工厂生产。

用处：当产品对象需要进行不同的加工时可以考虑工厂方法。

注意：这不是所谓的简单工厂的升级版，两者有不同的应用场景。

我们继续写一个枪工厂：

`interface Shootable{ shoot(); }`
`abstract class Gun implements Shootable { abstract shoot(); }`
`class AK47 extends Gun{ shoot () { console.log('ak47 shoot') }`
`class M4A1 extends Gun { shoot () { console.log ('m4a1 shoot ') } }`
//抽象枪工厂
`abstract class GunFactory { abstract create():Gun; }`
//创建ak47工厂
`class AK47Factory extends GunFactory {`
`create () : Gun {`
`let gun = new AK47(); `//生产ak47
` console.log('produce ak47 gun');`
`this.clean(gun);` //清理工作
`this.applyTungOil(gun);`//ak47涂上桐油
`return gun;`
`}`
`private clean ( gun : Gun) { console.log('清理ak');}`
`private applyTungOil(gun : Gun) { console.log('涂上桐油');}`
`}`
// 创建m4工厂
`class M4A1Factory extends GunFactory {`
`create ():Gun {`
//生产m4
`let gun = new M4A1();`
`console.log('produce m4a1 gun');`
`this.clean(gun);` //同样清理m4
`this.sprayPaint ( gun) ;` //M4是土豪金
`return gun;`
`}`
`private class ( gun : Gun) { console.log('清理m4‘);}`
`private class ( gun : Gun) { console.log(‘涂上土豪金');}`
`}`
//调用ak和m4工厂
`let ak47 = new AK47Factory().create();`
`ak47.shoot();`
//输出
produce ak47 gun; 清理ak；涂上桐油；
`let m4a1 = new M4A1Factory().create();`
`m4a1.shoot();`
//输出
produce m4a1 gun; 清理m4；涂上土豪金；

**可以看到ak和m4在生产出来后的处理不一样，ak涂桐油，m4喷土豪金。在简单工厂比较难做到，所以就每个产品都弄个工厂来封装各自己的生产过程。加一个awm就直接加个产品和产品工厂就好了。**
缺点：增加一个产品就需要多加两个类，增加了代码的复杂性。
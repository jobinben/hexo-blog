---
title: 关于JavaScript数组的sort()的详解
date: 2019-05-24 21:34:07
tags:
---
**sort()是对数组排序的一个api，对原有数组元素进行位置排序，关于排序有几种方法，以下列出几种简单常用的方法供给大家参考：**

## 1、简单数组排序
```javascript
	var arr = new Array(1,4,2,5,3);
	arr.sort();
	console.log("arr = ", arr.join());//输出 1,2，3,4，5； join的方法是把数组按默认的逗号拼接成一个字符串
```

## 2、简单数组的自定义排序
```javascript
	var arr = new Array(1,4,2,5,3);
	arr.sort(function(a,b) {
		return a - b;
	});
	console.log("arr = ", arr.join());//输出 1,2，3,4，5,；按小到大的排序；当b - a时；数组将会按大到小的排序；
```

## 3、简单对象列表（List）自定义属性排序
<!--more-->
```javascript
	var arrList = new Array();
	function Persion(name，age) {
		this.name = name;
		this.age = age;
	}
	arrList.push(new Persion('jobin',20));
	arrList.push(new Persion('jack', 19));
	arrList.push(new Persion('mery',25));
	//按年龄从小到大排序
	arrList.sort(function(a,b) {
		return a.age - b.age;
	});
	console.log(arrList);
	for(var i = 0; i < arrList.length; i++) {
		console.log("age:" + arrList[i].age + "name:" +arrList[i].name);
	} //输出<br>age:19name:jack
		// <br>age:20name:jobin
		// <br>age:25name:mery
```
## 4、简单对象列表（List）对可编辑属性的排序
```javascript
	var editList = new Array();
	function editWork(name,age) {
		this.name = name;
		var _age = age;
		this.age = function() {
			if(!arguments) {
				_age = arguments[0];
			} else {
				return _age;
			}
		}
	}
	editList.push(new editWork('jobin',22));
	editList.push(new editWork('jack', 19));
	editList.push(new editWork('mery',25));
	//还是按年龄从小到大排序
	editList.sort(function(a,b) {
		return a.age() - b.age();
	});
	console.log(editList);
	for(var i = 0; i < editList.length; i++){
		console.log("name:"+editList[i].name+"age:"+editList[i].age()); //输出name:jackage:19
//name:jobinage:22
// name:meryage:25
	}
```

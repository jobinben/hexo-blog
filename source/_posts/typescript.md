---
title: TypeScript设计模式之观察者模式
date: 2019-03-17 20:51:18
tags:
---

今天我们来学习一下TypeScript设计模式中的观察者模式！

**观察者模式：观察者模式上定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所以依赖于它的对象都得到通知并被自动更新。**

## 观察者模式的结构

 - Client : 客户端
 - Subject: 通知者
 - Observer: 观察者

## 观察者模式的利弊
利：

 - 观察者和被观察者是抽象耦合的。
 - 建立一套触发机制。
 
 <!-- more-->
 
 弊：
 
 - 如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。
 - 如果在观察者和观察者目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。

------------------------------**接下来话不多说，直接上码**---------------------------------

## 关于观察模式的例子

以下的例子是我最近在发现的一个Typescript事件通知机制，用到的模式是观察者模式：
```javascript


/**
 * s事件
 */
class Events {
    //监听数组
    private static listeners = {};

    /**
     * 注册事件
     * @param name 事件名称
     * @param callback 回调函数
     * @param context 上下文
     */
    public static register(name: string, callback: Function, context: any) {
        let observers: Observer[] = Events.listeners[name];
        if (!observers) {
            Events.listeners[name] = [];
        }
        Events.listeners[name].push(new Observer(callback, context));
    }

    /**
     * 移除事件
     * @param name 事件名称
     * @param callback 回调函数
     * @param context 上下文
     */
    public static remove(name: string, callback: Function, context: any) {
        let observers: Observer[] = Events.listeners[name];
        if (!observers) {
            return "remove";
        }
        let length = observers.length;
        for (let i = 0; i < length; i++) {
            let observer = observers[i];
            if (observer.compar(context)) {
                observers.splice(i,1);
                break;
            }
        }

        if (observers.length == 0) {
            delete Events.listeners[name];
        }
    }

    public static emit(name: string, ...args: any[]) {
        let observers: Observer[] = Events.listeners[name];
        if (!observers) {
            return "emit";
        }
        let length = observers.length;
        for (let i = 0; i < length; i++) {
            let observer = observers[i];
            observer.notify(name, ...args);
            
        }
    }
}

//观察者
class Observer {
    //回调函数
    private callback: Function = null;
    //上下文
    private context: any = null;

    constructor(callback: Function, context: any) {
        let self = this;
        self.callback = callback;
        self.context = context;
    }

    /**
     * 发送通知
     * @param args 不定参数
     */
    notify(...args: any[]):void {
        let self = this;
        self.callback.call(self.context, ...args);
    }

    /**
     * 上下文比较
     * @param context 上下文
     */
    compar(context: any): boolean {
        return context == this.context;
    }
}
```

测试代码：

```javascript

class Test {
    constructor () {
        Events.register("hello",this.test,self);
    }

    public test (eventName:string, args1: string, args2: number) {
        console.log(eventName,args1,args2);
        console.log(arguments);
    }
}

let a = new Test();
Events.emit("hi","hhh",1);
```
---
title: 手写深拷贝
date: 2019-05-21 13:23:38
tags:
---
说到深拷贝，大家应该第一反应就是这两个方法：**JSON.parse()** 和 **JSON.stringify()**
当然，这种方法需要保证对象是JSON的安全，所以只适用于部分情况。
**话不多说，直接上码：**
```javascript
	
    function deepCopy(obj)｛
        //判断是否是简单数据类型
        if(typeof obj == "object") {
            //复杂数据类型
            var result = obj.constructor == Array ? [] : {};
            for (let i in obj) {
                result[i] = typeof obj[i] == "object" ? deepCopy(obj[i]) : obj[i];
            }
        } else {
            //简单数据类型直接赋值
            var result = obj;
        }
        return result;
    ｝
```
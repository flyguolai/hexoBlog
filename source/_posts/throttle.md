---
title: 函数节流
date: 2020-07-07 23:25:00
tags: 编程常见方式
---

# 函数节流

函数节流是主要取消一段时间内的多次调用

## 目的

在一定时间内，比如一秒内只调用一次，然后下一秒内再调用一次

## 实现

具体的实现方式，其实是使用的一个变量作为开关，以控制节流的是否处罚

```javascript
// 时间戳版
function throttle(func, wait) {
    let previous = 0;
        return function() {
        let now = Date.now();
        let context = this;
        let args = arguments;
        if (now - previous > wait) {
            func.apply(context, args);
            previous = now;
        }
    }
}

content.onmousemove = throttle(count,1000);
```
这段代码[参考来源](https://www.cnblogs.com/cc-freiheit/p/10827372.html)

第一个参数为方法，为节流时调用的函数

第二个参数为防抖时间，用于防抖的延迟时间
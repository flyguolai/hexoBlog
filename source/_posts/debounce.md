---
title: 函数防抖
date: 2020-07-07 23:25:00
tags: 编程常见方式
---

# 函数防抖

短时间内多次触发同一事件，只执行一次

## 目的

只执行最后一次，或者只执行最开始的一次，中间的不执行。

## 实现

```javascript
// 合成版
/**
   * @desc 函数防抖
   * @param func 目标函数
   * @param wait 延迟执行毫秒数
   * @param immediate true - 立即执行， false - 延迟执行
   */
function debounce(func, wait, immediate) {
    let timer;
    return function() {
        let context = this,
            args = arguments;
            
        if (timer) clearTimeout(timer);
        if (immediate) {
            let callNow = !timer;
            timer = setTimeout(() => {
                timer = null;
            }, wait);
            if (callNow) func.apply(context, args);
        } else {
            timer  = setTimeout(() => {
                func.apply(context, args)
            }, wait)
        }
    }
}
```
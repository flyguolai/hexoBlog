---
title: 链表
date: 2020-10-12 12:14:10
tags: 数据结构
---

# 链表

链表是一个非常经典的数据结构，其本身只记录下一个数据的指针的特性导致其在做增删节点的时候效率非常高

它本身和数组明显的区别

- 数组：可以根据偏移实现快速的随机读写。
- 链表：扩容，增删元素快。

## 如何判断环形链表

[大神题解](https://leetcode-cn.com/problems/linked-list-cycle/solution/yi-wen-gao-ding-chang-jian-de-lian-biao-wen-ti-h-2/)

自我总结：

链表的很多东西都可以通过双指针的形式去进行求解（定位指针，探索指针）

在该题中利用单双数的特性，将定位指针定为步进1，探索指针定为步进2，如果链表为环形链表，步进2的指针肯定会有时候会与步进1的指针指向同一地址

解题答案

``` javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

// listNode 双指针法
// 一个指针每次走一步，另外一个指针每次走两步
// 进行循环
// 跳出循环条件为
// 1、两步节点的next为null（非环形）
// 2、两布节点的next与单步节点的next相同（环形）

/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    let a = b = head;

    while(a && b){
        if(b){
            b = b.next
            if(b){
                b = b.next
            }

            if(a === b){
                return true
            }else{
                a = a.next
            }
        }
    }

    return false;
};
```

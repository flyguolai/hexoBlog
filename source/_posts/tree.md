---
title: 树结构练习题
date: 2020-10-12 13:48:40
tags: 数据结构
---

## 树

类似于链表，不过从链表的一对一结构变化为了一对N的结构



### 二叉树

二叉树为一个根节点最多有两个叶子节点的结构

二叉树的主要遍历行为为

- 前序遍历
- 中序遍历
- 后续遍历

其对应的结构为，左叶子节点永远在右叶子节点前面，变化的产物主要为根节点的顺序

根节点在最前，为前序遍历，根节点在最后为后续遍历，根节点在中为中序遍历

#### 搜索二叉树的最小差值

思路：搜索二叉树为有序二叉树，其值在二叉树中有序排列，从小到大依次为左叶子，根节点，右叶子，所以可以直接获得其顺序，并可以确定 根-左 和 右-根 一定为正数，同时去除结构中的空叶子节点

通过prev和min全局变量存储之前的计算结果

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */


let prev = null
let min = null

// 使用中序遍历
// 中序遍历将二叉搜索树转换为有序数组，然后通过数组比较求出最小值
var middle = function(treeNode){
    if(treeNode){
        middle(treeNode.left)
        if(prev){
            if(min){
                min = Math.min(min,treeNode.val - prev.val)
            }else{
                min = treeNode.val - prev.val
            }
        }
        prev = treeNode
        
        middle(treeNode.right)
    }
}

/**
 * @param {TreeNode} root
 * @return {number}
 */
var getMinimumDifference = function(root) {
    prev = null;
    min = null;
    middle(root)
    return min
};
```
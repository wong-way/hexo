---
title: leetcode-404
date: 2018-06-29 23:48:50
categories: leetcode
tags:
- oj 
- leetcode
- binary tree
description: leetcode404. Sum of Left Leaves
---
# leetcode

## 问题描述

>Find the sum of all left leaves in a given binary tree.
>```text
>Example:
>
>    3
>   / \
>  9  20
>    /  \
>   15   7
>
>There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
>```

## 解题思路

这道题关键在于如何判断一个节点是左叶节点，不光要满足叶节点同时要是左节点。
* 判断叶节点可以通过判断他左右子节点是否为空得出
* 那么判断左节点只能通过该节点的父节点来判断
综上可知，如果满足`root.left!=null && root.left.left == null && root.left.right == null`则证明是左子节点

## 解题代码

```java
    public int sumOfLeftLeaves(TreeNode root) {
        if(root ==null) return 0;
        if (root.left!=null && root.left.left == null && root.left.right == null)
            return root.left.val + sumOfLeftLeaves(root.right);
        return sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);
    }
```
---
title: leetcode-513
date: 2018-06-17 15:04:38
categories: leetcode
tags:
- leetcode
- oj
description: leetcode No513. Find Bottom Left Tree Value
---
# leetcode No513. Find Bottom Left Tree Value\

## 问题描述

>Given a binary tree, find the leftmost value in the last row of the tree.
>```text
>Example 1:
>Input:
>
>   2
>   / \
>  1   3
>
>Output:
>1
>Example 2: 
>Input:
>
>       1
>      / \
>    2   3
>   /   / \
>  4   5   6
>     /
>    7
>
>Output:
>7
>```

## 解题思路

由于这道题不仅要找到最左边的元素，而且限定了是最顶层的最左边元素，因此需要判断左右子树哪一个更深来进行递归计算

## 解题代码

```java
public int findBottomLeftValue(TreeNode root) {
    if (root.left == null && root.right == null) {
        return root.val;
    }
    int left = getDepth(root.left);
    int right = getDepth(root.right);
    if (left > right) {
        return findBottomLeftValue(root.left);
    } else return findBottomLeftValue(root.right);
}
private int getDepth(TreeNode root) {
    if (root == null) return 0;
    else return 1 + Math.max(getDepth(root.left), getDepth(root.right));
}
```

## 问题回顾

上面这种方法我在写的时候就发现，对每一节点，先要递归计算左右子树的高度，再递归每一层每个节点，速度很慢。结果真的是这样，速度值超过了1%，于是改进思路

## 评论区优质解答

这种方法比较巧妙，通过全局变量`h`记录，第一次达到第`n`层的节点的值，保证了`h`中存的永远是第`n`层最左边的元素

```java
public int findBottomLeftValue1(TreeNode root) {
    findBottomLeftValue(root, 1);
    return ans;
}
public void findBottomLeftValue(TreeNode root, int depth) {

    if (h<depth) {ans=root.val;h=depth;}
    if (root.left!=null) findBottomLeftValue(root.left, depth+1);
    if (root.right!=null) findBottomLeftValue(root.right, depth+1);

}
```
---
title: leetcode-669
date: 2018-06-20 21:44:44
categories: leetcode
tags:
- leetcode
- oj
- binary tree
description: leetcode No669. Trim a Binary Search Tree
---
# leetcode No669. Trim a Binary Search Tree

## 问题描述

>Given a binary search tree and the lowest and highest boundaries as L and R, trim the tree so that all its elements lies in [L, R] (R >= L). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.
>```text
>Example 1:
>Input:
>    1
>   / \
>  0   2
>
>  L = 1
>  R = 2
>
>Output:
>    1
>      \
>       2
>Example 2:
>Input:
>    3
>   / \
>  0   4
>   \
>    2
>   /
>  1
>
>  L = 1
>  R = 3
>
>Output:
>     3
>     /
>   2
>  /
> 1
>```

## 解题思路

这里特别是二叉检索树采用递归解决，前序遍历:

* 如果根节点满足条件,则递归左右子树分别赋给左右子节点
* 如果小于区间最小值，则递归右子树赋给根节点
* 如果大于区间最大值，则递归左子树赋给根节点

## 解题代码

```java
public TreeNode trimBST(TreeNode root, int L, int R) {
    if (root == null) return null;
    if (root.val <= R && root.val >= L) {
        root.left = trimBST(root.left, L, R);
        root.right = trimBST(root.right, L, R);
    } else if (root.val > R) {
        root = trimBST(root.left, L, R);
    } else {
        root = trimBST(root.right, L, R);
    }
    return root;
}
```
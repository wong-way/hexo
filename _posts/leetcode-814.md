---
title: leetcode-814
date: 2018-06-10 14:04:25
categories: leetcode
tags: 
- leetcode
- oj
- binary tree
description: leetcode No.814 Binary Tree Pruning
---
# leetcode No.814  Binary Tree Pruning

## 问题描述

>We are given the head node root of a binary tree, where additionally every node's value is either a 0 or a 1.
Return the same tree where every subtree (of the given tree) not containing a 1 has been removed.
(Recall that the subtree of a node X is X, plus every node that is a descendant of X.)
>```text
>Example 1:
>Output: [1,null,0,null,1]
>Explanation: 
>Only the red nodes satisfy the property "every subtree not containing a 1".
>The diagram on the right represents the answer.
>Example 2:
>Input: [1,0,1,0,0,0,1]
>Output: [1,null,1,null,1]
>Example 3:
>Input: [1,1,0,1,1,0,1,0]
>Output: [1,1,0,1,1,null,1]
>```

## 解题思路

这道题典型的用递归解决，采用前序遍历，先判断自身，然后分别对左右子树进行处理

## 解题代码

```java
public TreeNode pruneTree(TreeNode root) {
    if (!containOne(root.left))
        root.left = null;
    else{
        pruneTree(root.left);
    }
    if (!containOne(root.right))
        root.right = null;
    else{
        pruneTree(root.right);
    }
    return root;
}

/**
* 判断子树中是否有1
*
* @param root 根节点
* @return 包含1则返回真
*/
public boolean containOne(TreeNode root) {
    if (root == null)
        return false;
    if (root.val == 1)
        return true;
    return containOne(root.right) || containOne(root.left);
}
```

## 改进解法

采用后序遍历

```java
public TreeNode pruneTree1(TreeNode root) {
    if(root == null)
        return root;
    root.left = pruneTree1(root.left);
    root.right = pruneTree1(root.right);
    if (root.left == null && root.right == null && root.val == 0) return null;
    return root;
}
```
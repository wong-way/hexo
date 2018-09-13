---
title: leetcode-700
date: 2018-08-23 22:28:03
categories: leetcode
tags: 
- leetcode
- oj
- binary tree
description: leetcode No700. Search in a Binary Search Tree
---
# leetcode No700. Search in a Binary Search Tree

## 问题描述
>Given the root node of a binary search tree (BST) and a value. You need to find the node in the BST that the node's value equals the given value. Return the subtree rooted with that node. If such node doesn't exist, you should return NULL.
>For example
>```text
>Given the tree:
>        4
>       / \
>      2   7
>     / \
>    1   3
>
>And the value to search: 2
>You should return this subtree:
>
>      2     
>    / \   
>   1   3
>```
>In the example above, if we want to search the value 5, since there is no node with value 5, we >should return NULL.
>
>Note that an empty tree is represented by NULL, therefore you would see the expected output (serialized tree format) as [], not null.

## 解题思路

直接递归遍历BST就行

## 解题代码

```java
public TreeNode searchBST(TreeNode root, int val) {
    if(root == null||root.val == val) return root;
    else if (root.val<val) return searchBST(root.right,val);
    else return searchBST(root.left,val);
}
```


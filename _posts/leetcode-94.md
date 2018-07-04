---
title: leetcode-94,leetcode-145,leetcode-144
date: 2018-06-25 15:41:20
categories: leetcode
tags:
- oj 
- leetcode
- binary tree
description: leetcode No94. Binary Tree Inorder Traversal、 No144.Binary Tree Preorder Traversal、No145. Binary Tree Postorder Traversal
---
# leetcode No94. Binary Tree Inorder Traversal

## 问题描述

>Given a binary tree, return the inorder traversal of its nodes' values.
>```text
>Example:
>
>Input: [1,null,2,3]
>   1
>    \
>    2
>   /
>  3
>
>Output: [1,3,2]
>```

## 问题分析

递归函数中序遍历，基本操作

## 解题代码

```java
class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> inorderTraversal(TreeNode root) {
        helper(root);
        return result;
    }
    private void helper(TreeNode root){
        if(root == null)
            return ;
        helper(root.left);
        result.add(root.val);
        helper(root.right);
    }
}

class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        helper(root);
        return result;
    }
    private void helper(TreeNode root){
        if(root== null) return;
        result.add(root.val);
        helper(root.left);
        helper(root.right);
    }
}
class Solution {
    List<Integer> result = new ArrayList<>();
    public List<Integer> postorderTraversal(TreeNode root) {
        helper(root);
        return result;
    }
    private void helper(TreeNode root){
        if(root== null) return;
        helper(root.left);
        helper(root.right);
        result.add(root.val);
    }
}
```
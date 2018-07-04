---
title: leetcode-617
date: 2018-05-07 10:52:17
categories: leetcode
tags:
- leetcode
- oj
- binary tree 
description: leetcode 第617题，Merge Two Binary Trees，难度简单
---
# leetcode-617 Merge Two Binary Trees

## 问题描述
>Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.
You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.
Example 1:

```text
Input: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
Output: 
Merged tree:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
```

>Note: The merging process must start from the root nodes of both trees.

## 问题分析

遍历二叉树有前序遍历，中序遍历，后序遍历，层次遍历等方法，我拟采用前序遍历的方式（中-左-右）解决这道题
* 首先，将根节点的值相加
* 第二，计算左子树
* 最后，计算右子树

## 解题代码

```java
public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
    TreeNode head ;
    if(t1 ==null&&t2==null)
        head = null;
    else if(t1==null)
        head= t2;
    else if(t2==null)
        head= t1;
    else
    {
        head = new TreeNode(t1.val + t2.val);
        head.left = mergeTrees(t1.left, t2.left);
        head.right = mergeTrees(t1.right, t2.right);
    }
    return head;
}
```

## 改良方法

```java
public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
    TreeNode head ;
    if(t1==null)
        head= t2;
    if(t2==null)
        head= t1;
    head = new TreeNode(t1.val + t2.val);
    head.left = mergeTrees(t1.left, t2.left);
    head.right = mergeTrees(t1.right, t2.right);
    return head;
}
```
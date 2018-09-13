---
title: leetcode-701
date: 2018-08-23 22:28:18
categories: leetcode
tags: 
- leetcode
- oj
- binary tree
description: leetcode No701. Insert into a Binary Search Tree
---
# leetcode No701. Insert into a Binary Search Tree

## 问题描述

Given the root node of a binary search tree (BST) and a value to be inserted into the tree, insert the value into the BST. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Note that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.

For example, 
```text
Given the tree:
        4
       / \
      2   7
     / \
    1   3
And the value to insert: 5
You can return this binary search tree:

         4
       /   \
      2     7
     / \   /
    1   3 5
This tree is also valid:

         5
       /   \
      2     7
     / \   
    1   3
         \
          4
```

## 解题思路
由于这里没有值重复的点，因此只要考虑插入即可。根据二叉检索树的特点，从根节点出发，值比根节点大的节点在右子树中，值比根节点小的节点在左子树中。利用这个特点，找到合适的位置，将节点插入即可。

## 解题代码
```java
public TreeNode insertIntoBST(TreeNode root, int val) {
    if(val<root.val){
        if(root.left == null) root.left = new TreeNode(val);
        else  insertIntoBST(root.left,val);
    }else{
        if(root.right ==null) root.right = new TreeNode(val);
        else insertIntoBST(root.right,val);
    }
    return root;
}
```

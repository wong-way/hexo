---
title: leetcode226
date: 2017-08-22 19:35:18
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第226道 Invert Binary Tree    
---

## leetcode\_top100\_226

* 问题描述

  >Invert a binary tree.
  >
  >```
  >     4
  >   /   \
  >  2     7
  > / \   / \
  >1   3 6   9
  >```
  >
  >to
  >
  >```
  >     4
  >   /   \
  >  7     2
  > / \   / \
  >9   6 3   1
  >```

* 问题分析

  反转二叉树，没什么好讲的。按照前序遍历的方式，先对根节点进行反转，然后递归，对左右子树进行反转即可。

* 解题代码

  ```java
  class Solution {
      public TreeNode invertTree(TreeNode root) {
          if(root==null)
              return null;
          TreeNode node = root.left;
          root.left = root.right;
          root.right = node;
          invertTree(root.left);
          invertTree(root.right);
          return root;
      }
  }
  ```

  ​
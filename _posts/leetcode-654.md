---
title: leetcode-654
date: 2018-06-03 10:33:14
categories: leetcode
tags:
- oj 
- leetcode
- binary tree
description: leetcode第654题，Maximum Binary Tree
---
# leetcode-654 Maximum Binary Tree

## 问题描述

>Given an integer array with no duplicates. A maximum tree building on this array is defined as follow:
The root is the maximum number in the array.
The left subtree is the maximum tree constructed from left part subarray divided by the maximum number.
The right subtree is the maximum tree constructed from right part subarray divided by the maximum number.
Construct the maximum tree by the given array and output the root node of this tree.

```text
Example 1:
Input: [3,2,1,6,0,5]
Output: return the tree root node representing the following tree:

      6
    /   \
   3     5
    \    /
     2  0
       \
        1
```

## 问题分析

这道题，第一反应就是写一个辅助函数，构造`[i,j]`区间的最大树，然后递归构造左子树和右子树

## 解题代码

```java
public TreeNode constructMaximumBinaryTree(int[] nums) {
    return constructSubtree(0, nums.length - 1, nums);
}

public TreeNode constructSubtree(int start,int end,int[]nums){

    if(start > end)
        return null;
    int index =start;
    TreeNode root ;
    for(int i = start ;i<=end;i++) {
        if (nums[i] > nums[index]) {
            index = i;
        }
    }
    root = new TreeNode(nums[index]);
    root.left = constructSubtree(start, index - 1, nums);
    root.right = constructSubtree(index + 1, end, nums);
    return root;

}
```

评论区的老哥的思路大多数都是像我这样，这种运行速度也是最快的，超过了98%的提交记录

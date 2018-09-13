---
title: leetcode-230
date: 2018-08-02 10:18:07
categories: leetcode
tags: 
- leetcode
- oj
- binary search
description: leetcode 230. Kth Smallest Element in a BST
---
# leetcode 230. Kth Smallest Element in a BST

## 问题描述

>Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.
>
>Note: 
>You may assume k is always valid, 1 ≤ k ≤ BST's total elements.
>```text
>Example 1:
>
>Input: root = [3,1,4,null,2], k = 1
>   3
>  / \
> 1   4
>  \
>   2
>Output: 1
>Example 2:
>
>Input: root = [5,3,6,2,4,null,null,1], k = 3
>       5
>      / \
>     3   6
>    / \
>   2   4
>  /
> 1
>Output: 3

## 解题思路
1. 计算比当前节点小的节点个数`smallCount`。
    * 如果`smallCount = k-1`，则当前节点即为所求，直接返回
    * 如果`smallCount > k-1`,说明所求的节点位于当前节点的左子树中，递归计算左子树
    * 如果`smallCount > k-1`,说明所求的节点位于当前节点的右子树中,递归计算右子树，这里需要注意，由于右边已经有`smallCount`个小于该节点值的节点，则传入的参数`k`需要改变，`k=k-smallCount-1`
## 解题代码

```java
public int kthSmallest(TreeNode root, int k) {

    if(root == null) return 0;
    int smallerCount = getTreeCount(root.left);
    if(smallerCount == k-1) return root.val;
    else if(smallerCount > k-1){
        return kthSmallest(root.left,k);
    }else{
            return kthSmallest(root.right,k-1-smallerCount);
    }
    
}
private int getTreeCount(TreeNode root){
    if(root ==null) return 0;
    int count = 1;
    count=count+getTreeCount(root.left)+getTreeCount(root.right);
    return count;
}
```
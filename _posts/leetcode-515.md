---
title: leetcode-515
date: 2018-06-23 18:04:06
categories: leetcode
tags:
- leetcode
- oj
- binary tree
description: leetcode No515. Find Largest Value in Each Tree Row(binary tree)
---
# leetcode

## 问题描述

>You need to find the largest value in each row of a binary tree.
>```text
>Example:
>Input:
>
>          1
>         / \
>        3   2
>       / \   \  
>      5   3   9 
>
>Output: [1, 3, 9]

## 解题思路

编写一个辅助函数，`getLevelLargest(TreeNode node, int level)`,`level`是node所在的层数，通过判断`node.val`与列表里存的数比较，大则更新，小则跳过

## 解题代码

```java
List<Integer> result = new ArrayList<>();

public List<Integer> largestValues(TreeNode root) {
    getLevelLargest(root, 1);
    return  result;
}

private void getLevelLargest(TreeNode node, int level) {
    if (node == null) return ;
    if(level>result.size())
        result.add(node.val);
    else if (node.val > result.get(level - 1)) {
        result.set(level - 1, node.val);
    }
    getLevelLargest(node.left, level + 1);
    getLevelLargest(node.right, level + 1);
}
```
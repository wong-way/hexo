---
title: leetcode-538
date: 2018-06-29 22:04:35
categories: leetcode
tags:
- oj 
- leetcode
- binary tree
description: leetcodeNo.Convert BST to Greater Tree
---
# leetcodeNo.Convert BST to Greater Tree

## 问题描述

>Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.
>```text
>Example:
>
>Input: The root of a Binary Search Tree like this:
>              5
>            /   \
>           2     13
>
>Output: The root of a Greater Tree like this:
>            18
>           /   \
>         20     13
>```

## 解题思路

我们需要把每个节点都加上大于它的值的节点，由于这是一个二叉检索树大于它的值的节点一定在它的右边，那么我采用一种特殊的遍历方式，利用右中左的遍历顺序。一开始我写的辅助函数为` private void helper(TreeNode root,int addNum)`用addNum记录要加上的数字，后来我直接改成`private void helper(TreeNode root)`,而使用一个全局的`addNum`来记录比当前节点大的节点的值的总和，这样更加清晰直观，而不需要在每次调用函数的时候去计算`addNum`是多少。

## 解题代码

```java
class Solution {
    int addNum = 0;
    public TreeNode convertBST(TreeNode root) {
        helper(root);
        return root;
      
    }
    private void helper(TreeNode root){
        if(root ==null) return;
        helper(root.right);
        int temp ;
        temp = root.val+addNum;
        addNum+=root.val;

        root.val = temp;
        helper(root.left);
    }
}
```

## 评论区

```java
public class Solution {
    int sum = 0;
    
    public TreeNode convertBST(TreeNode root) {
        if (root == null) return null;
        
        convertBST(root.right);
        
        root.val += sum;
        sum = root.val;
        
        convertBST(root.left);
        
        return root;
    }
}
```

```java
    private int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if (root == null) return null;
        convertBST(root.right);
        int tmp = root.val;
        root.val += sum;
        sum += tmp;
        convertBST(root.left);
        return root;
    }
```

评论区高票代码跟我完全一样，到现在2018-06-29 22:13刷了这么多道oj，深切的感受到自己对数据结构理解的加深，代码技巧上的提高。保研就要临近了，不知道自己能去一个什么样的大学，还没准备的事情很多，但又不知从何做起，感觉最近懒了不少，该把要做的都赶紧做完好好准备了，希望有一个好的结果吧。
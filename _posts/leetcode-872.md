---
title: leetcode-872
date: 2018-08-02 10:18:35
categories: leetcode
tags: 
- leetcode
- oj
- depth first search
description: leetcode No872.Leaf-Similar Trees
---
# leetcode No872.Leaf-Similar Trees

## 问题描述
>Consider all the leaves of a binary tree.  From left to right order, the values of those leaves form a leaf value sequence.
>```text
>         3
>       /   \
>      5     1
>     / \   / \
>    6  2  9   8
>      / \
>     7   4
>```
>For example, in the given tree above, the leaf value sequence is (6, 7, 4, 9, 8).
>Two binary trees are considered leaf-similar if their leaf value sequence is the same.
>Return true if and only if the two given trees with head nodes root1 and root2 are leaf-similar.

## 解题思路
对两棵二叉树分别进行获取叶节点的操作，然后在比较叶节点的值是否相同

## 解题代码

```java
    public boolean leafSimilar(TreeNode root1, TreeNode root2) {
       
       List<Integer> list1 = new ArrayList<>();
       getLeafValue(list1,root1);   
       List<Integer> list2 =  new ArrayList<>();
       getLeafValue(list2,root2);
       return isSame(list1,list2);
      
    }
    private void getLeafValue(List<Integer> list,TreeNode root){
        if(root==null) return;
        if(root.left == null && root.right == null)
            list.add(root.val);
        else
        {
            getLeafValue(list,root.left);  
            getLeafValue(list,root.right);
        }   

    }
    private boolean isSame(List<Integer> list1,List<Integer> list2){
        if(list1.size()!=list2.size()) return false;
        for(int i = 0;i<list1.size();i++){
            if(list1.get(i)!=list2.get(i)) return false;
        }
        return true;
    }
```
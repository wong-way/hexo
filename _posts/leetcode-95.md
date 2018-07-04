---
title: leetcode-95
date: 2018-06-29 21:11:37
categories: leetcode
tags:
- oj
- leetcode
- binary tree
description: leetcode No95. Unique Binary Search Trees II
---
# leetcodeNo95. Unique Binary Search Trees II

## 问题描述

>Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1 ... n.
>```text
>Example:
>
>Input: 3
>Output:
>[
>  [1,null,3,2],
>  [3,2,null,1],
>  [3,1,null,null,2],
>  [2,1,3],
>  [1,null,2,null,3]
>]
>Explanation:
>The above output corresponds to the 5 unique BST's shown below:
>
>   1         3     3      2      1
>    \       /     /      / \      \
>     3     2     1      1   3      2
>    /     /       \                 \
>   2     1         2                 3
>```

## 解题思路

解题思路那就递归呗。
首先，遍历一遍所有可能的根节点，然后将小于他的值递归，并作为它的左子树。大于它的值递归作为它的右子树。

## 解题代码

```java
public class Solution95 {

    public List<TreeNode> generateTrees(int n) {

        return helper(generateList(1, n));
    }

    private List<TreeNode> helper(List<Integer> list) {
        List<TreeNode> nodeList = new ArrayList<>();
        if (list.size() == 0) {
            return nodeList;
        }
        for (int i = 0; i < list.size(); i++) {

            List<TreeNode> left = helper(list.subList(0, i));
            List<TreeNode> right = helper(list.subList(i + 1, list.size()));
            if (left.size() != 0 && right.size() != 0) {
                for (TreeNode l : left) {
                    for (TreeNode r : right) {
                        TreeNode node = new TreeNode(list.get(i));
                        node.left = l;
                        node.right = r;
                        nodeList.add(node);
                    }
                }
            } else if (left.size() != 0) {
                for (TreeNode l : left) {
                    TreeNode node = new TreeNode(list.get(i));
                    node.left = l;
                    node.right = null;
                    nodeList.add(node);
                }
            } else if (right.size() != 0) {
                for (TreeNode r : right) {
                    TreeNode node = new TreeNode(list.get(i));
                    node.left = null;
                    node.right = r;
                    nodeList.add(node);
                }

            } else {
                TreeNode node = new TreeNode(list.get(i));
                nodeList.add(node);
            }
        }
        return nodeList;
    }

  

    /**
     * 根据传入的参数low,high 生成列表，包含low,包含high
     *
     * @param low  下界
     * @param high 上界
     * @return 生成的列表
     */
    private List<Integer> generateList(int low, int high) {
        List<Integer> list = new ArrayList<>();
        for (int i = low; i <= high; i++) {
            list.add(i);
        }
        return list;
    }

    public static void main(String[] args) {
        Solution95 solution95 = new Solution95();
        List<TreeNode> list = solution95.generateTrees(3);
        for (TreeNode node : list) {
            System.out.println(node.val);
        }
    }

}

```
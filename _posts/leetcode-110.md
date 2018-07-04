---
title: leetcode_110
date: 2017-08-07 16:20:54
tags: leetcode
categories: leetcode
description: leetcode算法题第110道Balanced Binary Tree
---

## Balanced Binary Tree

>Given a binary tree, determine if it is height-balanced.
>
>For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of *every* node never differ by more than 1.

给出一个二叉树的节点，验证它是不是平衡二叉树。

平衡二叉树的定义：

>a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of *every* node never differ by more than 1

**解决思路**

我的想法是计算每个节点左右子树的高度，计算他们的高度差，看他们是否满足平衡二叉树的定义

**解题代码**

```java
public class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;
        int left = depth(root.left);
        int right = depth(root.right);
        return (Math.abs(left-right)<=1)&&(isBalanced(root.left))&&(isBalanced(root.right));
    }
    public int depth(TreeNode root){
    	if(root == null) return 0;
    	return Math.max(depth(root.left),depth(root.right))+1;
    }
}
```

这个算法的存在一个问题，那就是对每个节点都要进行递归，这样递归的次数多时间开销就比较大。

**最优解法**

```java
public boolean isBalanced(TreeNode root) {
        return helper(root) != -1;
    }
    public int helper(TreeNode root){
        if (root == null) return 0;
        int left = helper(root.left);
        int right = helper(root.right);
        if (left == -1 || right == -1 || Math.abs(left - right) > 1) return -1;
        return Math.max(left, right) + 1;
    }
	public int dfs(TreeNode root){
        if (root == NULL) return 0;
    
        int leftHeight = dfsHeight (root -> left);
        if (leftHeight == -1) return -1;
        int rightHeight = dfsHeight (root -> right);
        if (rightHeight == -1) return -1;
        
        if (abs(leftHeight - rightHeight) > 1)  return -1;
        return max (leftHeight, rightHeight) + 1;
	}
}
```

`helper`这个方法最终结果如果返回的是`-1`那就不是平衡树。采用深度优先的思想，如果一棵树不是平衡树，那么以他的父结点为根节点的二叉树一定也不是平衡树。这种方法时间复杂度为O(n)，显然更好。
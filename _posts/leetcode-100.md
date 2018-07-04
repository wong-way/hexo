---
title: leetcode_100
date: 2017-07-12 22:48:01
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第100道 Same Tree
---

### leetcode\_100\_Same Tree

> Given two binary trees, write a function to check if they are equal or not.
>
> Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

**问题分析**

​	这道题，让我们判断两个二叉树是否相等。看到这个题目，我马上想到肯定用递归来解决，采用遍历来解决这个问题，那么遍历二叉树的方法有几种，中序遍历，前序遍历，后序遍历，在这个地方采用哪个遍历方式最后呢？最后我采用了**前序遍历**，前序遍历有什么好处了，如果在根节点就不相同那么直接返回，就不用再递归调用子节点，节省了一部分时间和空间。

**解题思路**

* 前序遍历
* 对终止迭代的一些条件进行定义

**解题代码**

```java
public class Solution100 {
    public static boolean isSameTree(TreeNode p, TreeNode q) {
        boolean isSame = false;
        if (p == null && q == null) return true;
        else if (p == null && q != null) return false;
        else if (p != null && q == null) return false;
        if (p.val != q.val)
            isSame = false;
        else
            isSame = ((isSameTree(p.left, q.left)) && (isSameTree(p.right, q.right)));
        return isSame;
    }
    public static void main(String[] args) {
        TreeNode l11 = new TreeNode(2);
        TreeNode l21 = new TreeNode(3);
        TreeNode l22 = new TreeNode(5);
        TreeNode l31 = new TreeNode(6);
        l11.left = l21;
        l11.right = l22;
        l21.left = l31;

        TreeNode n11 = new TreeNode(2);
        TreeNode n21 = new TreeNode(3);
        TreeNode n22 = new TreeNode(5);
        TreeNode n31 = new TreeNode(6);
        n11.left = n21;
        n11.right = n22;
        n21.left = n31;

        boolean isSame = isSameTree(l11, n11);
        System.out.println(isSame);
    }
}
```

**示例代码**

```java
public boolean isSameTree(TreeNode p, TreeNode q) {
    if(p == null && q == null) return true;
    if(p == null || q == null) return false;
    if(p.val == q.val)
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    return false;
}
```

不得不服气，自己写代码的能力和别人还是有很大的差距，同样是前序遍历，他这段代码各种条件的判断都十分有讲究。例如

```java
if(p == null && q == null) return true;
if(p == null || q == null) return false;
```

这两行代码其实和我的代码运行结果是一样的，但是我重复考虑了`p==null&&q == null`的情况，他这种方式更加简洁。

```java
if(p.val == q.val)
 	return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
```

这两行代码同样也是这个问题，我考虑`p.val ! =q.val`的情况，以及`p.val == q.val`都做了处理，其实是没有必要的。
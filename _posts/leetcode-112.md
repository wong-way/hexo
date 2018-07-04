---
title: leetcode_112
date: 2017-07-14 15:48:49
categories: leetcode
tags:
- leetcode
- oj
description: leetcode No.112 Path Sum 
---

> Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.
>
> For example:
>
> Given the below binary tree and 

```
sum = 22
```

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```

> return true, as there exist a root-to-leaf path `5->4->11->2` which sum is 22.

这道题要求我们求出是不是存在一条路径，满足从根节点到叶节点所有的值加起来。这个问题也是可以用递归来解决的。

**解题思路**

* 首先检查`root == null`的情况，返回`false`
* 判断终止递归的条件，即在叶节点时`if(num == root.val)`。
* 递归，传入的参数为`num - root.val`，判断左子树或者右子树是否满足。

**解题代码**

```java
public class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root==null)
        	return false;
        if(root.val == sum &&root.left==null&&root.right== null)
        	return true;
        if((hasPathSum(root.left,sum-root.val))||(hasPathSum(root.right,sum-root.val)))
        	return true;
        return false;
    }
}
```


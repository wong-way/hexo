---
title: leetcode_104-111
date: 2017-07-14 15:26:23
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第104 Maximum Depth of Binary Tree
---

### leetcode\_104\_Maximum Depth of Binary Tree

> Given a binary tree, find its maximum depth.
>
> The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

这道题让我们求二叉树的最大高度，采用递归的方式皆可以很轻松的解决这道题。

**解题思路**

* 判断递归终止条件`root == null`，则`return 0`；
* 进行递归。返回左右子树的最大层数加一，`return 1+max(leftHeight,rightHeight)`。

```java
 public int maxDepth(TreeNode root) {
       if(root == null)
            return 0;
        else{
            int left = maxDepth(root.left);
            int right = maxDepth(root.right);            
            return 1+(left>right?left:right);
        }
 }
```

### leetcode\_111\_Minimum Depth of Binary Tree

> Given a binary tree, find its minimum depth.
>
> The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

这道题让我们求出二叉树的最小的深度，如果我们按照上一道题的思路来做，结果是正确的吗？

答案是否定。

主要的问题是出现在这里`return 1+(left<right?left:right);`，返回左右子树中高度较小的。

如果出现一个内部节点只有左子树或者右子树的时候，那么左右子树中高度较小的肯定是0。这样就不能保证是从根节点到叶节点的最小高度。因此我们在返回的时候需要对这种情况进行检查。

**解题思路**

* 判断递归终止条件`root == null`，则`return 0`

* 如果左右子树都存在的情况下，返回左右子树中较小的高度

  ```java
  if (leftDepth > 0 && rightDepth > 0)
  {
    return 1 + (leftDepth > rightDepth ? rightDepth : leftDepth);
  }
  ```

* 如果不满足上诉条件，则证明只存在左子树或者右子树，因此就要返回左子树或者右子树相应的高度。

  ```java
  return 1 + (leftDepth < rightDepth ? rightDepth : leftDepth);
  ```

**解题代码**

```java
public class Solution {
    public int minDepth(TreeNode root) {
        if (root == null)
            return 0;
        else {
            int leftDepth = minDepth(root.left);
            int rightDepth = minDepth(root.right);
            if (leftDepth > 0 && rightDepth > 0)
                return 1 + (leftDepth > rightDepth ? rightDepth : leftDepth);
            return 1 + (leftDepth < rightDepth ? rightDepth : leftDepth);
        }
    }
}
```


---
title: leetcode_189
date: 2017-07-26 11:08:03
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第1899道 Rotate Array
---

## leetcode\_189\_Rotate Array

>Rotate an array of *n* elements to the right by *k* steps.
>
>For example, with *n* = 7 and *k* = 3, the array `[1,2,3,4,5,6,7]` is rotated to `[5,6,7,1,2,3,4]`.
>
>**Note:**
>Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
>
>**Hint:**
>Could you do it in-place with O(1) extra space?

1. ## **问题分析**

这道题让我们将数组的每个元素向右移动k位，利用一个辅助数组来做比较简单。

将所有情况分为两类，一类是`i+k<len`,直接赋值给赋值元素，一类是`i+k>len`，这种情况就对结果取余就行了

2. ## **解题代码**

```java
public class Solution {
    public void rotate(int[] nums, int k) {
        int []result = new int[nums.length];
        for(int i = 0;i<nums.length;i++){
           result[(i+k)%nums.length] = nums[i];   
        }
       for(int i = 0;i<nums.length;i++){
           nums[i] = result [i];
       }
    }
}
```


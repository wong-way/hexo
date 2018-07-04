---
title: leetcode-26
date: 2018-07-02 16:48:51
categories: leetcode
tags:
- leetcode
- oj
- two pointer
description: leetcode No80.Remove Duplicates from Sorted Array II
---
# leetcode No80.Remove Duplicates from Sorted Array II

## 问题描述
>Given a sorted array nums, remove the duplicates in-place such that duplicates appeared at most twice and return the new length.
>
>Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.
>
>Example 1:
>
>Given nums = [1,1,1,2,2,3],
>
>Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.
>
>It doesn't matter what you leave beyond the returned length.
>Example 2:
>
>Given nums = [0,0,1,1,1,1,2,3,3],
>
>Your function should return length = 7, with the first seven elements of nums being modified to 0, 0, 1, 1, 2, 3 and 3 respectively.
>
>It doesn't matter what values are set beyond the returned length.

## 问题分析

整体思路是通过两个指针，一个遍历整个数组，一个用来记录新的下标。如果满足`num[i]>nums[index-2]`这说明需要更新

## 解题代码

```java
public int removeDuplicates(int[] nums) {
    if(nums.length==0)
        return 0;
    int count = 2;      
    for (int i = 2; i < nums.length; i++) {
        if (nums[i] !=nums[count-2]){
            nums[count++] = nums[i];
        }
    
    }
    return count;
}
```
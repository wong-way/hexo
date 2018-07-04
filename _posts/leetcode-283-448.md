---
title: leetcode-283/448
date: 2018-05-09 21:13:12
categories: leetcode
tags: 
- oj
- leetcode
- arrays
description: leetcode第283题（Move Zeroes）、448题（Find All Numbers Disappeared in an Array）
---
# leetcode-283（Move Zeroes），leetcode-448（Find All Numbers Disappeared in an Array）

## leetcode-283（Move Zeroes）

### 问题描述

>Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.
For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].
Note:
You must do this in-place without making a copy of the array.
Minimize the total number of operations.
Credits:

### 问题分析

利用插入排序的思想，遍历整个数组，将每个元素`插入`到应该处于的位置。与插入排序不同的是，插入排序需要将元素插入到特定的位置，而我们只需要依次按顺序插入即可。

### 解决代码

```java
public void moveZeroes1(int[] nums) {
    int pos = 0;
    for (int i = 0; i < nums.length; i++) {
        if(nums[i]!=0){
            nums[pos++] = nums[i];
        }
    }
    while(pos<nums.length){
        nums[pos++]=0;
    }
}
```

## leetcode-448（Find All Numbers Disappeared in an Array）

### 问题描述

>Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.
Find all the elements of [1, n] inclusive that do not appear in this array.
Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.
Example:
```
Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
```

### 问题分析

The basic idea is that we iterate through the input array and mark elements as negative using nums[nums[i] -1] = -nums[nums[i]-1]. In this way all the numbers that we have seen will be marked as negative. In the second iteration, if a value is not marked as negative, it implies we have never seen that index before, so just add it to the return list.

### 解题代码

```java
public List<Integer> findDisappearedNumbers(int[] nums) {
    List<Integer> ret = new ArrayList<Integer>();

    for(int i = 0; i < nums.length; i++) {
        int val = Math.abs(nums[i]) - 1;
        if(nums[val] > 0) {
            nums[val] = -nums[val];
        }
    }

    for(int i = 0; i < nums.length; i++) {
        if(nums[i] > 0) {
            ret.add(i+1);
        }
    }
    return ret;
}
```

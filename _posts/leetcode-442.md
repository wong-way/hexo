---
title: leetcode-442
date: 2018-05-31 23:59:57
categories: leetcode
tags:
- leetcode
- oj 
- arrays
description: leetcode第442题Find All Duplicates in an Array
---
# leetcode-442 Find All Duplicates in an Array

## 问题描述
>iven an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.
Find all the elements that appear twice in this array.
Could you do it without extra space and in O(n) runtime?
**Example**

```text
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
```
## 解题思路

* 方法一

时间复杂度O(nlogn)
将数组排序，遍历数组，比较每一位与前一位是否相同，如果相同，则说明重复。

* 方法二 hash

利用空间代价换时间，申请一个新的数组当做字典，将数组中的值作为键。遍历数组，对每一个数组中的值`x`，令dict[x]=1,如果在后续遍历过程中发现dict[x] ==1,则说明重复

## 解题代码

```java
public List<Integer> findDuplicates(int[] nums) {
    Arrays.sort(nums);
    List list = new ArrayList();
    for(int i = 1;i<nums.length;i++){
        if(nums[i]==nums[i-1])
            list.add(nums[i]);
    }
    return list;
}

public List<Integer> findDuplicates1(int[] nums) {
    int[] dict = new int[nums.length];
    List list = new ArrayList();
    for (int i = 0; i < nums.length; i++) {
        if (dict[nums[i]] == 0) {
            dict[nums[i]] =1;
        }else {
            list.add(nums[i]);
        }
    }
    return list;
}

```
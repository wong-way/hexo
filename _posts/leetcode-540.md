---
title: leetcode-540
date: 2018-06-18 11:03:23
categories: leetcode
tags:
- oj
- leetcode
description: leetcode No540.Single Element in a Sorted Array
---
# leetcode No540.Single Element in a Sorted Array

## 问题描述

>Given a sorted array consisting of only integers where every element appears twice except for one element which appears once. Find this single element that appears only once.
>```text
>Example 1:
>Input: [1,1,2,3,3,4,4,8,8]
>Output: 2
>Example 2:
>Input: [3,3,7,7,10,11,11]
>Output: 10
>Note: Your solution should run in O(log n) time and O(1) space.
>```

## 解题思路

这道题要时间复杂度为`log n`，那么一定要采用二分查找的方法。我整体思路如下，对区间[i,j]取中间索引`mid`的值，判断其与左右的关系，同时，判断左右`[i,mid]`,`[mid,j]`的长度得出该单值出现在哪个区间，同时将`[i,j]`区间缩小

## 解题代码

```java
public int singleNonDuplicate(int[] nums) {
    int end = nums.length-1;
    int start =0;
    int mid = (end+start)/2;
    while(end>start){
        int  left = nums[mid-1];
        int right = nums[mid+1];
        if(nums[mid] == left)
        {
            if((mid-start+1)%2 ==1){
                end = mid-2;
            }else{
                start =mid+1;
            }
        }
        else if(nums[mid]==right){
            if((end-mid+1)%2 ==1){
                start = mid+2;
            }else{
                end =mid-1;
            }
        }
        else{
            return nums[mid];
        }
        mid = (end+start)/2;
    }
    return nums[start];
}
```
---
title: leetcode-16
date: 2018-05-22 17:37:49
categories: leetcode
tags:
- leetcode 
- oj 
- two pointer
description: leetcode第16题，3Sum Closest（Two pointer 问题）
---
# leetcode-16 3Sum Closest

## 问题描述

>Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.
**Example**

```text

Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

## 问题分析

这道题如果用暴力破解的方式进行解决，对每一种方式进行处理，那么时间复杂度为O(n^3)
我准备采用O(n^2)的方式来解决这个问题，对数组中的每一个参数，采取双指针向左右扩散的方式

## 解题代码

```java
public int threeSumClosest(int[] nums, int target) {
    Arrays.sort(nums);
    int result = Integer.MAX_VALUE+target;
    for (int i = 1; i < nums.length; i++) {
        int temp = getSum(nums, i, target);
        if (Math.abs(temp - target) < Math.abs(result - target)) {
            result = temp;
        }

    }
    return result;

}

public int getSum(int[] nums, int index, int target) {
    int i = index-1;
    int j = index+1;
    int sum = Integer.MAX_VALUE+target;
    while (i >= 0 && j < nums.length) {
        int temp = nums[i] + nums[index] + nums[j];
        //System.out.println(index + " " + i + " " + j + " " + temp);
        if (Math.abs(temp - target) < Math.abs(sum - target)) {
            sum = temp;
        }
        if (temp > target) {
            i--;
        } else if (temp<target){
            j++;
        }else return target;
    }
    return sum;
}
```

### 讨论区做法

```java
public class Solution {
    public int threeSumClosest(int[] num, int target) {
        int result = num[0] + num[1] + num[num.length - 1];
        Arrays.sort(num);
        for (int i = 0; i < num.length - 2; i++) {
            int start = i + 1, end = num.length - 1;
            while (start < end) {
                int sum = num[i] + num[start] + num[end];
                if (sum > target) {
                    end--;
                } else {
                    start++;
                }
                if (Math.abs(sum - target) < Math.abs(result - target)) {
                    result = sum;
                }
            }
        }
        return result;
    }
}
```
评论区这个做法，仿佛跟我一模一样！先用Arrays.sort排序，这一招是我之前做题学到的，然后采用双指针处理。感觉自己做了那么久的题还是有掌握了一些技巧的。
---
title: leetcode-304
date: 2018-04-22 22:43:09
categories: leetcode
tags:
- leetcode
- oj
- dynamic programming  
description: leetcode第303题，Range Sum Query
---
# leetcode-303 Range Sum Query

## 问题描述

>Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.
Example:
>>Given nums = [-2, 0, 3, -5, 2, -1]
sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3

## 问题分析

由于`sumRange`会多次调用，那么我们就应该尽量降低它的时间复杂度，同时由于构造函数只有一次，那么他的时间复杂度稍微高一些也可以。
因此，我将NumArray初始化的操作计算为`S[n]=a[0]+..a[n]`，前n项和。在`sumRange`中计算`a[n]+...+a[m]=S[m]-S[n-1]`。
总结起来这道题就是一道数列的前n项和与第n项之间的关系。

## 解题代码

```java
int[] sumArr;
public NumArray(int[] nums) {
    for(int i = 1; i < nums.length; i++)
        nums[i] += nums[i - 1];
    this.sumArr = nums;
}

public int sumRange(int i, int j) {
    if(i==0)
        return sumArr[j];
    return sumArr[j]-sumArr[i-1];
}
```

## 优质解答

```java
private int[] sum;

public NumArray(int[] nums) {
    sum = new int[nums.length + 1];
    for (int i = 0; i < nums.length; i++) {
        sum[i + 1] = sum[i] + nums[i];
    }
}

public int sumRange(int i, int j) {
    return sum[j + 1] - sum[i];
}
```
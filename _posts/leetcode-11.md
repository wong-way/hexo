---
title: leetcode-11
date: 2018-05-05 10:40:24
categories: leetcode
tags:
- leetcode 
- oj 
- arrays
description: leetcode第十一题-container with most water，难度中等
---
# leetcode-11 Container With Most Water

## 问题描述

>Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.
Note: You may not slant the container and n is at least 2.

## 问题分析

* 这道题属于算法的典型最优化问题，一方面容器的宽度在发生变化，同时容器的高度也在随着不同的宽度变化而变化

* 一开始这道题肯定可以想到，列举每一个组合情况，计算出最大的容器

## 解题代码

```java
public int maxArea(int[] height) {
    int maxArea = 0;
    for (int i = 0; i < height.length; i++)
        for (int j = i + 1; j < height.length; j++)
            maxArea = Math.max(maxArea, Math.min(height[i], height[j]) * (j - i));
    return maxArea;
}
```

## 再思考

暴力破解的方式固然可以解决问题，但是存在效率不高的问题。于是改进算法，利用双指针向中间靠拢的方式。指针移动通过判断限制水桶的因素是什么来决定移动头指针还是尾指针。

## 解题代码2

```java
public int maxArea1(int[] height) {
    int left = 0, right = height.length - 1;
    int maxArea = 0;

    while (left < right) {
        maxArea = Math.max(maxArea, Math.min(height[left], height[right])
                * (right - left));
        if (height[left] < height[right])
            left++;
        else
            right--;
    }

    return maxArea;
}
```
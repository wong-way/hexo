---
title: leetcode-746
date: 2018-04-24 10:34:00
categories: leetcode
tags:
- leetcode
- oj
- dynamic programming
description: leetcode第746题，爬楼梯（动态规划问题）
---
# leetcode-746 Min Cost Climbing Stairs

## 问题描述

>On a staircase, the i-th step has some non-negative cost cost[i] assigned (0 indexed).
Once you pay the cost, you can either climb one or two steps. You need to find minimum cost to reach the top of the floor, and you can either start from the step with index 0, or the step with index 1.
>>Example 1:
Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.
>>Example 2:
Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[3].

## 问题分析

之前有一道计算爬楼梯的方式有多少种采用的是`dp[i]=dp[i-1]+dp[i-2]`动态规划的方式来解决。
此处，增加了每次消耗一定的体力值，要求我们利用最小的体力值爬完所有的楼梯。则有`pay[i]=min(pay[i-1]+cost[i-1],pay[i-2]+cost[i-2])`(pay[i]是指爬到第i层需要花费的最小体力值)

## 解题代码

```java
public int getMin(int x,int y){
    return x>y?y:x;
}

public int minCostClimbingStairs(int[] cost) {
    int len = cost.length;
    int[] pay = new int [len];
    for(int i = 2;i<cost.length;i++){
        pay[i] = getMin(pay[i-1]+cost[i-1],pay[i-2]+cost[i-2]);
    }
    return getMin(pay[len - 1]+cost[len-1], pay[len - 2]+cost[len-2]);
}
```

## 代码优化

```java
public int minCostClimbingStairs1(int[] cost) {
    for(int i = 2; i < cost.length; i++)
    {
        cost[i] = cost[i] + Math.min(cost[i-2], cost[i-1]);
    }
    return Math.min(cost[cost.length-1], cost[cost.length - 2]);
}
```

在上述代码中，通过计算后的`cost[i]`是指从第i层开始往下走需要花费的体力值，最终我们只要比较倒数第一层和倒数第二层往下走的体力值消耗，选出两者中较小的就可以了


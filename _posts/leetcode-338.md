---
title: leetcode-338
date: 2018-04-24 19:20:30
categories: leetcode
tags:
- leetcode
- oj
- dynamic programming
description: leetcode第338题，counting bits（动态规划）
---
# leetcodde-338 counting bits

## 问题描述

>Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.
Example:
For num = 5 you should return [0,1,1,2,1,2].
Follow up:
>* It is very easy to come up with a solution with run time O(n*sizeof(integer)). But can you do it in linear time O(n) /possibly in a single pass?
>* Space complexity should be O(n).
>* Can you do it like a boss? Do it without using any builtin function like __builtin_popcount in c++ or in any other language.

## 问题分析

这道题属于动态规划，那么动态规划的重点是要找出递推公式，此处我采用的递推公式为`dp[i] = dp[flag]+dp[i-flag]`,其中flag为2的整数次幂。
例如：`9 = 8 + 1 =>1001 = 1000+1`

## 解题代码

```java
public int[] countBits(int num) {
    int bits[] = new int[num + 1];
    int flag = 0;
    for (int i = 1; i <= num; i++) {
        if ((i & i - 1) == 0) {
            bits[i] = 1;
            flag = i;
        } else {
            bits[i] = bits[flag] + bits[i - flag];
        }
    }
    return bits;
}
```

## 问题总结

这次提交超过了85%的提交记录。15个测试记录消耗了3ms。
但是我在讨论区看到了其他的递推公式`dp[i] = dp[i/2] + (i&1);`,那么对应的可以使用位操作来解决这个问题`dp[i] = dp[i>>2] + (i&1);`
相应的好理解:
>` 9 => 1001 `
>` 4 => 100`
>` 1 => 1`

## 优化代码

```java
public int[] countBits1(int num) {

    int[] dp = new int[num+1];

    for(int i=1;i<num+1;i++)
        dp[i] = dp[i/2] + (i&1);

    return dp;

}
public int[] countBits2(int num){
    int[] dp = new int[num + 1];
    for (int i=1; i<=num; i++) dp[i] = dp[i >> 1] + (i & 1);
    return dp;
}
```
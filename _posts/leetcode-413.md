---
title: leetcode-413
date: 2018-04-26 14:31:59
categories: leetcode 
tags:
- leetcode 
- oj
- dynamic programming
description: leetcode 第413题 Arithmetic slices（动态规划） 运行速度超过100%提交记录
---
# leetcode-413 Arithmetic Slices

## 问题描述

>A sequence of number is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.
For example, these are arithmetic sequence:

```
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```

>The following sequence is not arithmetic.
```
1, 1, 2, 5, 7
```
>A zero-indexed array A consisting of N numbers is given. A slice of that array is any pair of integers (P, Q) such that 0 <= P < Q < N.
A slice (P, Q) of array A is called arithmetic if the sequence:
A[P], A[p + 1], ..., A[Q - 1], A[Q] is arithmetic. In particular, this means that P + 1 < Q.
The function should return the number of arithmetic slices in the array A.
Example:
```
A = [1, 2, 3, 4]
return: 3, for 3 arithmetic slices in A: [1, 2, 3], [2, 3, 4] and [1, 2, 3, 4] itself.
```

## 问题分析

这道题和回文子串非常相似，在回文子串中，如果`aba`是回文串，那么`xabax`一定是回文子串，在这里如果`1 2 3`是等差数列，同时如果第四个元素满足其条件，那么`1 2 3 a[4]`也是等差数列。基于以上思想计算出每个下标开头的等差数列的个数

## 解题代码

```java
public int numberOfArithmeticSlices(int[] A) {
    int len = A.length;
    int count = 0;
    int i = 0;
    while (i < len - 2) {
        count += isArithmetic(A, i);
        i++;
    }
    return count;
}

private int isArithmetic(int[] A, int start) {
    int i = 0;
    int count = 0;
    int step = A[start + 1] - A[start];
    if (step != A[start + 2] - A[start + 1]) {
        return 0;
    }
    i = start + 2;
    count = 1;
    while (i < A.length-1 && (A[i + 1] - A[i] == step)) {
        i++;
        count++;
    }
    return count;
}
```

## 深化理解

* 这道题用递归程序也肯定能够解决问题
```java
public class Solution {
    int sum = 0;
    public int numberOfArithmeticSlices(int[] A) {
        slices(A, A.length - 1);
        return sum;
    }
    public int slices(int[] A, int i) {
        if (i < 2)
            return 0;
        int ap = 0;
        if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
            ap = 1 + slices(A, i - 1);
            sum += ap;
        } else
            slices(A, i - 1);
        return ap;
    }
}
```
* 考虑一下动态规划
如果用`dp[i]`表示从第`i`个元素结束能够构成等差数列的个数，则状态转移方程为
`dp[i]=dp[i-1]+dp[i]-dp[i-1]==step?1:0`
```java
public class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int[] dp = new int[A.length];
        int sum = 0;
        for (int i = 2; i < dp.length; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                dp[i] = 1 + dp[i - 1];
                sum += dp[i];
            }
        }
        return sum;
    }
}
```

* 动态规划常量空间

```java
public class Solution {
    public int numberOfArithmeticSlices(int[] A) {
        int dp = 0;
        int sum = 0;
        for (int i = 2; i < A.length; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                dp = 1 + dp;
                sum += dp;
            } else
                dp = 0;
        }
        return sum;
    }
}
```

## 纪念一下
{% asset_img record.svg %}



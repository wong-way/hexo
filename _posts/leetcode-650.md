---
title: leetcode-650
date: 2018-06-14 18:00:07
categories: leetcode
tags:
- oj 
- leetcode
- dynamic programming
description: leetcode No650. 2 Keys Keyboard
---
# leetcode No650. 2 Keys Keyboard

## 问题描述
>Initially on a notepad only one character 'A' is present. You can perform two operations on this notepad for each step:
>
>Copy All: You can copy all the characters present on the notepad (partial copy is not allowed).
>Paste: You can paste the characters which are copied last time.
>Given a number n. You have to get exactly n 'A' on the notepad by performing the minimum number of steps permitted. Output the minimum number of steps to get n 'A'.
>```text
>Example 1:
>Input: 3
>Output: 3
>Explanation:
>Intitally, we have one character 'A'.
>In step 1, we use Copy All operation.
>In step 2, we use Paste operation to get 'AA'.
>In step 3, we use Paste operation to get 'AAA'.
>```

## 问题分析

如果最后有n个A，那么我将它进行因式分解`n=a*b(a>b)`,即我需要的n个A，可以通过将a个A复制b遍，或者相反。同时，最快的方法应该是将b个A复制a遍那么相应的递推公式`dp[n] = dp[a]+b`

## 解题代码

```java
public int minSteps(int n) {
    int[] dp = new int[n+1];

    for (int i = 2; i <= n; i++) {
        dp[i] = i;
        for (int j = i-1; j > 1; j--) {
            if (i % j == 0) {
                dp[i] = dp[j] + (i/j);
                break;
            }

        }
    }
    return dp[n];
}
```
---
title: leetcode-62
date: 2018-06-14 11:17:38
categories: leetcode
tags:
- oj 
- leetcode
- dynamic programming
description: leetcode No62.Unique Paths(dynamic programming)
---
# leetcode leetcode No62.Unique Paths

## 问题描述

>A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
How many possible unique paths are there?
>```text
>Example 1:
>
>Input: m = 3, n = 2
>Output: 3
>Explanation:
>From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
>1. Right -> Right -> Down
>2. Right -> Down -> Right
>3. Down -> Right -> Right
>Example 2:
>
>Input: m = 7, n = 3
>Output: 28
>```

## 问题分析

这道题典型的动态规划问题，`dp[i][j]`表示到第`i`行第`j`列递推公式为`dp[i][j] = dp[i-1][j]+dp[i][j-1]`

## 解题代码

```java
public int uniquePaths(int m, int n) {
    int len = m*n;
    int [] dp = new int[len];
    dp[0]=1;
    int  i= 1;
    while(i<len){
        int row = i/m;
        int col = i%m;
        if(row == 0){
            dp[i] = dp[col-1];
        }else if(col ==0){
            dp[i] = dp[(row-1)*m];
        }else{
            dp[i] = dp[i-1]+dp[(row-1)*m+col];
        }

        i++;
    }
    return dp[len-1];
}

public static void main(String[] args) {
    Solution62 solution = new Solution62();
    solution.uniquePaths(3, 7);
}
```
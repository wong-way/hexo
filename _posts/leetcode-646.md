---
title: leetcode-646
date: 2018-05-12 12:55:23
categories: leetcode
tags:
- leetcode
- oj
- dynamic programming
description: leetcode 第646题动态规划问题，Maximum Length of Pair Chain
---
# leetcode-646 Maximum Length of Pair Chain

## 问题描述

>You are given n pairs of numbers. In every pair, the first number is always smaller than the second number.
Now, we define a pair (c, d) can follow another pair (a, b) if and only if b < c. Chain of pairs can be formed in this fashion.
Given a set of pairs, find the length longest chain which can be formed. You needn't use up all the given pairs. You can select pairs in any order.

Example 1:

```text
Input: [[1,2], [2,3], [3,4]]
Output: 2
Explanation: The longest chain is [1,2] -> [3,4]
```

Note:
The number of given pairs will be in the range [1, 1000].

## 解题过程

### 解题思路

* 使用`dp[i]`表示以第`i`个元素结尾的最大长度
* 利用双层循环将每一种[i,j]的组合都计算一遍，计算出从`i`到`j`是不是满足条件，如果满足`d[j]=max(d[j],d[i]+1)`

### 解题代码

```java
public int findLongestChain(int[][] pairs) {
    if (pairs.length < 2)
        return pairs.length;
    Arrays.sort(pairs, (a, b) -> (a[0] - b[0]));
    int[] dp = new int[pairs.length];
    int max = 0;
    dp[0] = 1;
    for (int i = 0; i < pairs.length; i++) {
        for (int j = i+1; j < pairs.length; j++) {
            if(pairs[j][0]>pairs[i][1]){
                dp[j]=dp[j]>dp[i]+1?dp[j]:dp[i]+1;
            }else{
                dp[j]=dp[j]>0?dp[j]:1;
            }
        }
    }
    for (int i = 0; i < dp.length; i++) {
        if(max<dp[i])
            max = dp[i];
    }
    return max;
}
```

### 改进思路

使用`dp[i]`表示到第`i`个元素为止，最大的距离长度

```java
public int findLongestChain(int[][] pairs) {
    if (pairs == null || pairs.length == 0) return 0;
    Arrays.sort(pairs, (a, b) -> (a[0] - b[0]));
    int[] dp = new int[pairs.length];
    Arrays.fill(dp, 1);
    for (int i = 0; i < pairs.length; i++) {
        for (int j = i+1 j < pairs.length; j++) {
            dp[j] = Math.max(dp[j], pairs[j][0] > pairs[i][1]? dp[i] + 1 : dp[i]);
        }
    }
    return dp[pairs.length - 1];
}
```

### 贪心算法解决

待续
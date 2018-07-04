---
title: leetcode-486
date: 2018-06-13 17:18:27
categories: leetcode
tags:
- leetcode
- oj
- dynamic programming
description: leetcode No.486 Predict the Winner
---
# leetcode No486. Predict the Winner

## 问题描述

>Given an array of scores that are non-negative integers. Player 1 picks one of the numbers from either end of the array followed by the player 2 and then player 1 and so on. Each time a player picks a number, that number will not be available for the next player. This continues until all the scores have been chosen. The player with the maximum score wins.
Given an array of scores, predict whether player 1 is the winner. You can assume each player plays to maximize his score.
>```text
>Example 1:
>Input: [1, 5, 2]
>Output: False
>Explanation: Initially, player 1 can choose between 1 and 2. 
>If he chooses 2 (or 1), then player 2 can choose from 1 (or 2) and 5. If player 2 chooses 5, then player 1 will be left with 1 (or 2). 
>So, final score of player 1 is 1 + 2 = 3, and player 2 is 5. 
>Hence, player 1 will never be the winner and you need to return False.
>Example 2:
>Input: [1, 5, 233, 7]
>Output: True
>Explanation: Player 1 first chooses 1. Then player 2 have to choose between 5 and 7. No matter which number player 2 choose, player 1 can choose 233.
>Finally, player 1 has more score (234) than player 2 (12), so you need to return True representing player1 can win.
>Note:
>1 <= length of the array <= 20.
>Any scores in the given array are non-negative integers and will not exceed 10,000,000.
>If the scores of both players are equal, then player 1 is still the winner.
>```

## 解题思路

这道题属于动态规划的问题，动态规划最重要的是找到递推公式以及递推的含义。不同的递推可能有不同的解决思路。

这里我用`dp[i][j]`表示`[i,j]`区间中，player1分数与player2分数的差值，那么`dp[i][j] = max{nums[i] - dp[i+1][j],nums[j] - dp[i][j-1]}`,即player1选择`nums[i]`或者`nums[j]`时减去player2在`[i+1,j]`或者`[i,j-1]`区间比player1多得的分数

## 解题代码

```java
public boolean PredictTheWinner(int[] nums) {
    int dp[][] = new int[nums.length][nums.length];
    for (int i = 0; i < nums.length; i++) {

        dp[i][i] = nums[i];
    }
    for (int i = 1; i < nums.length; i++) {
        for (int j = i-1; j >=0; j--) {
            dp[i][j] = Math.max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1]);
        }
    }
    return dp[0][nums.length - 1] >= 0;
}
```

## 评论区解答

```java
public boolean PredictTheWinner(int[] nums) {
    if (nums == null) { return true; }
    int n = nums.length;
    if ((n & 1) == 0) { return true; } // Improved with hot13399's comment.
    int[] dp = new int[n];
    for (int i = n - 1; i >= 0; i--) {
        for (int j = i; j < n; j++) {
            if (i == j) {
                dp[i] = nums[i];
            } else {
                dp[j] = Math.max(nums[i] - dp[j], nums[j] - dp[j - 1]);
            }
        }
    }
    return dp[n - 1] >= 0;
}
```

这个方法比较巧妙，他通过两个循环来控制取值的区间，最后反复的替换，使得`dp[i]`的值为从`i`到`len`的player1与player2的差值
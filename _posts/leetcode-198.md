---
title: leetcode_198
date: 2017-08-07 18:28:55
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第198道 House Robber
---

## House Robber

**问题描述**

>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.
>
>Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

这道题是动态规划的一个例题，主要思想就是采用动态规划的方法，对每个房子进行偷或不偷的判断，最后求出最大值。动态规划的重点是找出递推公式。

> 递推公式
>
> `money[i][0] = max(money[i - 1][0], money[i - 1][1]) `
>
> 上述公式表示，不偷第 `i `家房子，当前最大收益就是前一家房子的最大收益，前一家房子可能被偷了，也可能没有被偷。 
>
> `money[i][1] = money[i - 1][0] + nums[i] `
>
> 上述公式表示，要偷第` i` 家的房子，必须在不偷第` i-1` 家房子的前提下，才能加上偷第 i 家获得的收益。

一般来说，给定一个规则，让我们求任意状态下的解，都是用动态规划。这里的规则是劫匪不能同时抢劫相邻的屋子，即我们在累加时，只有两种选择：

1. 如果选择不抢劫当前屋子，所以最大收益就是抢劫上一个屋子的收益或者不抢劫上一个屋子的收益，取他们最大值
2. 如果选择抢劫当前屋子，就不能抢劫上一个屋子，所以最大收益是到上一个屋子的上一个屋子为止的最大收益，加上当前屋子里有的钱

所以，我们只要判断一下两个里面哪个大就行了，同时也是我们的递推式。另外我们可以做一点优化，本来我们是要用一个dp数组来保存之前的结果的。**但是我们只需要记录是否抢劫当前屋子的收益最大值即可。**

**解题代码**

```java
public class Solution {
    public int rob(int[] nums) {
        int rob = 0; //max monney can get if rob current house
        int notrob = 0; //max money can get if not rob current house
        for(int i=0; i<nums.length; i++) {
            int currob = notrob + nums[i]; //if rob current value, previous house must not be robbed
            notrob = Math.max(notrob, rob); //if not rob ith house, take the max value of robbed (i-1)th house and not rob (i-1)th house
            rob = currob;
        }
        return Math.max(rob, notrob);
    }
}
```


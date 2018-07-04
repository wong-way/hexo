---
title: leetcode_122
date: 2017-07-21 22:14:07
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第122道 Best Time to Buy and Sell Stock II
---

### leetcode\_122\_ Best Time to Buy and Sell Stock II

> Say you have an array for which the *i*th element is the price of a given stock on day *i*.
>
> Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

这道题说我们多可以多次买入和卖出股票，那么则意味着我们只要能够盈利就可以买入，思路十分简单清晰

代码如下：

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int sum = 0;
        for(int i=1; i<prices.length ; i++){
            if(prices[i]-prices[i-1]>0)
                sum+= prices[i]-prices[i-1];
        }
         sum+=max;
        return sum;
    }
}
```


---
title: leetcode_121
date: 2017-07-17 00:18:33
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第121道 Best Time to Buy and Sell Stock
---

### leetcode\_121\_Best Time to Buy and Sell Stock

>Say you have an array for which the *i*th element is the price of a given stock on day *i*.
>
>If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.
>
>**Example 1:**
>
>```
>Input: [7, 1, 5, 3, 6, 4]
>Output: 5
>
>max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
>
>```
>
>**Example 2:**
>
>```
>Input: [7, 6, 4, 3, 1]
>Output: 0
>
>In this case, no transaction is done, i.e. max profit = 0.
>```

**问题分析**

这道题最直接最简单的思路，从前往后搜索最小值，从后往前搜索最大值，并且保证最大值下标大于最小值的下标。但是我注意到`prices[i]-prices[i-1]`即是一天的收益值，那么这个就和之前那个搜索最大连续子数组那个题目十分相似了。

**解题思路**

* 设置一个`temp`用来记录连续子串的和，`max`记录最大连续子串的和；
* 如果`temp>max`，那么就更新`max`
* 如果`temp<0`，则说明在当天已经出现亏损，则不会出售。继续往后循环。

**解题代码**

```java
public int maxProfit(int[] prices) {
        int max = 0;
        for(int i=1;i<prices.length;i++){
            temp+= prices[i]-prices[i-1];
            if(temp<0)
                temp=0;
            if(temp>max)
                max =temp;
        }
        return max;
}
```


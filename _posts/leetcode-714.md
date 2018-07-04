---
title: leetcode-714
date: 2018-05-20 15:12:45
categories: leetcode
tags:
- leetcode 
- oj
- dynamic programming
description: leetcode第714题，Best Time to Buy and Sell Stock with Transaction Fee(动态规划问题)
---
# leetcode-714 Best Time to Buy and Sell Stock with Transaction Fee

## 问题描述

>Your are given an array of integers prices, for which the i-th element is the price of a given stock on day i; and a non-negative integer fee representing a transaction fee.
You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction. You may not buy more than 1 share of a stock at a time (ie. you must sell the stock share before you buy again.)
Return the maximum profit you can make.

**Example 1:**

```text
Input: prices = [1, 3, 2, 8, 4, 9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
Buying at prices[0] = 1
Selling at prices[3] = 8
Buying at prices[4] = 4
Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

**Note:**

* 0 < prices.length <= 50000.
* 0 < prices[i] < 50000.
* 0 <= fee < 50000.

## 问题分析

我的思路是每一天对于股票只有两种方式，一种是拥有股票，另一种是没有股票。

* 如果这一天有股票
    * i-1次拥有，继续保留
    * i-1次没有，重新购入
* 如果这一天没有股票
    * i-1次拥有，卖出
    * i-1次没有，保持

## 解题代码

下面这种方式有待修正

```java
public int maxProfit(int[] prices, int fee) {
    int[] own = new int[prices.length];
    int[] notOwn = new int[prices.length];
    for (int i = 1; i < prices.length; i++) {
        //own[i]表示第i次结束拥有股票时最大利润
        // - i-1次拥有，继续保留
        // - i-1次没有，重新购入
        own[i] = Math.max(notOwn[i - 1] - prices[i], own[i - 1] + prices[i] - prices[i - 1]);
        //notOwn[i]表示第i次结束没有股票时最大利润
        // - i-1次拥有，卖出
        // - i-1次没有，保持
        notOwn[i] = Math.max(own[i - 1] + prices[i] - prices[i - 1] - fee, notOwn[i - 1]);
        System.out.println(own[i] + " " + notOwn[i]);

    }
    return Math.max(own[prices.length - 1], notOwn[prices.length - 1]);
}
```

修正版,空间O(n)

```java

public int maxProfit1(int[] prices, int fee) {
    int[] own = new int[prices.length];
    int[] notOwn = new int[prices.length];
    own[0] = -prices[0];
    for (int i = 1; i < prices.length; i++) {
        //own[i]表示第i次结束拥有股票时最大利润
        // - i-1次拥有，继续保留
        // - i-1次没有，重新购入
        own[i] = Math.max(notOwn[i - 1] - prices[i], own[i - 1]);
        //notOwn[i]表示第i次结束没有股票时最大利润
        // - i-1次拥有，卖出
        // - i-1次没有，保持
        notOwn[i] = Math.max(own[i - 1] + prices[i] - fee, notOwn[i - 1]);
        System.out.println(own[i] + " " + notOwn[i]);
    }
    return notOwn[prices.length - 1];
}
```

加强版，时间O(n),空间O(1)

```java
public int maxProfit2(int[] prices, int fee) {
    int own = -prices[0];
    int notOwn =0;

    for (int i = 1; i < prices.length; i++) {
        //own表示第i次结束拥有股票时最大利润
        // - i-1次拥有，继续保留
        // - i-1次没有，重新购入
        own = Math.max(notOwn - prices[i], own);
        //notOwn 表示第i次结束没有股票时最大利润
        // - i-1次拥有，卖出
        // - i-1次没有，保持
        notOwn = Math.max(own+ prices[i] - fee, notOwn);
        System.out.println(own + " " + notOwn);
    }
    return notOwn;
}
```

注意，在计算`notOwn`时`notOwn = max(notOwnPre,ownPre+prices[i]-fee)`,但是在上面的代码中，我们是直接利用`notOwn = max(notOwnPre,own+prices[i]-fee`.
证明如下：
```text
如果，ownPre>notOwnPre-prices[i]时，own = ownPre则满足以上条件
反之，own = notOwnPre-prices[i],则own+prices[i]-fee = notOwnPre-fee<notOwnPre,所以max(own+prices[i]-fee,notOwnPre) = notOwnPre
```
直观来想，由于有手术费的存在，在同一天买入和卖出是不如继续保持原状划算

## 正确方式

>At the end of the i-th day, we maintain cash, the maximum profit we could have if we did not have a share of stock, and hold, the maximum profit we could have if we owned a share of stock.
To transition from the i-th day to the i+1-th day, we either sell our stock cash = max(cash, hold + prices[i] - fee) or buy a stock hold = max(hold, cash - prices[i]). At the end, we want to return cash. We can transform cash first without using temporary variables because selling and buying on the same day can't be better than just continuing to hold the stock.

```java
public int maxProfit(int[] prices, int fee) {
    int cash = 0, hold = -prices[0];
    for (int i = 1; i < prices.length; i++) {
        cash = Math.max(cash, hold + prices[i] - fee);
        hold = Math.max(hold, cash - prices[i]);
    }
    return cash;
}
```
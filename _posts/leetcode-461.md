---
title: leetcode-461
date: 2018-04-29 09:24:44
categories: leetcode
tags:
- oj
- leetcode
description: leetcode第461题，Hamming Distance，难度简单
---
# leetcode-461 Hamming Distance

## 问题描述

>The Hamming distance between two integers is the number of positions at which the corresponding bits are different.
Given two integers x and y, calculate the Hamming distance.
Note:
0 ≤ x, y < 2^31.
Example:

```text
Input: x = 1, y = 4
Output: 2
Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
The above arrows point to positions where the corresponding bits are different.
```

## 问题分析

这道题让我们计算出两个三十二位整型数字之间，有多少位不同。直接使用异或操作，然后计算出结果中`1`的个数。

## 解题代码

```java
public int hammingDistance(int x, int y) {
    int result = x^y;
    int count=0;
    for (int i = 0; i < 32; i++) {
        count+= result&1;
        result>>=1;
    }
    return count;
}
```
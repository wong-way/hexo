---
title: leetcode_172
date: 2017-07-26 10:23:51
categories: leetcode
tags:
- oj
- leetcode
description: leetcode第172题，Factorial Trailing Zeroes
---

## leetcode\_172\_Factorial Trailing Zeroes

>Given an integer *n*, return the number of trailing zeroes in *n*!.
>
>**Note: **Your solution should be in logarithmic time complexity.

1. ## **问题分析**

这道题要求我们在O(log)内完成，计算出阶乘末尾有多少个0，注意阶乘结果有0，是由2*5得到的。由于每次隔五个数才会出现5的倍数，而数字2则是隔两个数就会出现一次。所以2的个数比5的个数多，因此我们只需要计算在数字n之前，有多少个数字是5的倍数，即可求出有多少个0。

2. ## **解题代码**

```java
public class Solution {
    public int trailingZeroes(int n) {
      int count = 0;
      while(n>0){
        count+= n/5;
        n/=5;
      }
      return count;
    }
}
```


---
title: leetcode_136
date: 2017-07-23 14:33:23
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第136道 single number
---

## leetcode\_136\_single number

> Given an array of integers, every element appears *twice* except for one. Find that single one.
>
> **Note:**
> Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

这道题看起来很容易是不是，就只是遍历整个数组记录他们每个数字出现的次数。但是在提示中说到，时间复杂度要求是O(n)，同时不能使用额外的内存空间。

看到这道题我想了很久，根本没有什么好的思路，后来在讨论区看到提示才恍然大悟。 异或！

在一堆数字中，`a^b^c^d^c^b^a`由于异或的可交换性，可变成`a^a^b^b^c^c^d`。由于异或的特点，一个数与它本身异或则得到0,0和一个数字异或得到这个数字。那么`a^a^b^b^c^c^d= d`，即只存在一次的数字

代码

```java
public class Solution {
    public int singleNumber(int[] nums) {
        int result = 0;
        for(int i = 0;i<nums.length; i++){
            result ^=nums[i];
        }
        return result;
    }
}
```


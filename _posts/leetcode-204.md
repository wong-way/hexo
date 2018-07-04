---
title: leetcode-204
date: 2018-04-18 15:55:42
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode第204题，count primes
---
# leetcode 204 count primes

## Description

>Count the number of prime numbers less than a non-negative number, n.

## Analysis

这道题让我们计算0-n范围内的所有质数的个数

* 首先质数的定义：质数定义为在大于1的自然数中，除了1和它本身以外不再有其他因数。
* 如果我们这道题对每个数字都做质数定义的验证，那么时间复杂度将为O(n^2)，必定会导致超时
* 我们时间有限制，那么我们就用空间来换，利用一个数组记录一个数是否为质数。如果是则将计数器加一，同时他的所有属于[0,n]范围内的倍数，都不是质数

## Solution

```java
public int countPrimes(int n) {
    boolean[] notPrime = new boolean[n];
    int count = 0;
    for (int i = 2; i < n; i++) {
        if (notPrime[i] == false) {
            count++;
            for (int j = 2; i*j < n; j++) {
                notPrime[i*j] = true;
            }
        }
    }
    return count;
}
```

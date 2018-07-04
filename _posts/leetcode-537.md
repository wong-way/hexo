---
title: leetcode-537
date: 2018-06-10 21:03:47
categories: leetcode
tags:
- leetcode 
- oj
description: leetcode No.537 Complex Number Multiplication
---
# leetcode No.537 Complex Number Multiplication

## 问题描述

>Given two strings representing two complex numbers.
>You need to return a string representing their multiplication. Note i2 = -1 according to the definition.
>```text
>Example 1:
>Input: "1+1i", "1+1i"
>Output: "0+2i"
>Explanation: (1 + i) * (1 + i) = 1 + i2 + 2 * i = 2i, and you need convert it to the form of 0+2i.
>Example 2:
>Input: "1+-1i", "1+-1i"
>Output: "0+-2i"
>Explanation: (1 - i) * (1 - i) = 1 + i2 - 2 * i = -2i, and you need convert it to the form of 0+-2i.
>```

## 解题代码

```java
public String complexNumberMultiply(String a, String b) {

    int realNum1 = Integer.parseInt(a.substring(0, a.indexOf('+')));
    int imaginaryNum1 = Integer.parseInt(a.substring(a.indexOf('+') + 1, a.indexOf('i')));

    int realNum2 = Integer.parseInt(b.substring(0, b.indexOf('+')));
    int imaginaryNum2 = Integer.parseInt(b.substring(b.indexOf('+') + 1, b.indexOf('i')));

    int realNum = realNum1 * realNum2 - imaginaryNum1 * imaginaryNum2;
    int imaginaryNum = realNum1 * imaginaryNum2 + realNum2 * imaginaryNum1;

    return realNum + "+" + imaginaryNum + "i";
}
```
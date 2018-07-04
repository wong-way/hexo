---
title: leetcode-791
date: 2018-06-11 10:06:49
categories: leetcode
tags:
- leetcode
- oj
- string
description: leetcode No.791  Custom Sort String
---
# leetcode No.791  Custom Sort String

## 问题描述

>S and T are strings composed of lowercase letters. In S, no letter occurs more than once.
>
>S was sorted in some custom order previously. We want to permute the characters of T so that they match the order that S was sorted. More specifically, if x occurs before y in S, then x should occur before y in the returned string.
>
>Return any permutation of T (as a string) that satisfies this property.
>```text
>Example :
>Input: 
>S = "cba"
>T = "abcd"
>Output: "cbad"
>Explanation: 
>"a", "b", "c" appear in S, so the order of "a", "b", "c" should >be "c", "b", and "a". 
>Since "d" does not appear in S, it can be at any position in T. "dcba", "cdba", "cbda" are also valid outputs.
>```

## 解题思路

这道题分两种情况进行处理：

* 先处理在`S`中的字符，如果他们在`T`中出现，那么先将他们连接到字符串中
* 再处理没有出现在`S`中的字符，直接将他们连接的字符串的尾部

## 解题代码

```java
public String customSortString(String S, String T) {
    char[] arr = S.toCharArray();
    String result = "";
    for (char ch :
            arr) {
        for (int i = 0; i < T.length(); i++) {
            if(T.charAt(i)==ch)result += ch;
        }

    }
    for (int i = 0; i < T.length(); i++) {
        if (S.indexOf(T.charAt(i))==-1) {
            result += T.charAt(i);
        }
    }
    return result;
}
```
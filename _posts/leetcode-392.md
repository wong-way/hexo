---
title: leetcode-392
date: 2018-06-26 20:28:03
categories: leetcode
tags:
- oj 
- leetcode
- greedy
description: leetcode No392. Is Subsequence(greedy)
---
# leetcode

## 问题描述

>Given a string s and a string t, check if s is subsequence of t.
>
>You may assume that there is only lower case English letters in both s and t. t is potentially a very long (length ~= 500,000) string, and s is a short string (<=100).
>
>A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ace" is a subsequence of "abcde" while "aec" is not).
>
>Example 1:
>s = "abc", t = "ahbgdc"
>
>Return true.
>
>Example 2:
>s = "axc", t = "ahbgdc"
>
>Return false.

## 解题思路

使用双指针的方法，一个指针记录已经匹配的个数，一个指针遍历t

## 解题代码

```java
public boolean isSubsequence(String s, String t) {
    int index = 0;
    char[] arr = s.toCharArray();
    char[] dict = t.toCharArray();

    for(int i = 0;i<dict.length&&index<arr.length;i++){
        if (arr[index] == dict[i]) {
            index ++;
        }
    }
    return index == arr.length;
}
```

## 评论区优质解答

```java
public boolean isSubsequence(String s, String t) {
    if (s.length() == 0) return true;
    int indexS = 0, indexT = 0;
    while (indexT < t.length()) {
        if (t.charAt(indexT) == s.charAt(indexS)) {
            indexS++;
            if (indexS == s.length()) return true;
        }
        indexT++;
    }
    return false;
}
```

其实和我的方法没什么区别，嘻嘻
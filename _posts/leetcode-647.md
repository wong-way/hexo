---
title: leetcode-647
date: 2018-04-26 10:33:31
categories: leetcode
tags: 
- leetcode
- oj 
- dynamic programming
description: leetcode第647题，Palindromic Substring(动态规划)
---
# leetcode-647 palindromic substring

## 问题描述

>Given a string, your task is to count how many palindromic substrings in this string.
The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.
>>Example 1:
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
>>Example 2:
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".

## 问题分析

这道题是`算法竞赛入门经典`中给出的例题，推荐的动态规划方法是从中间节点向两边延伸

## 解题代码

```java
public int countSubstrings(String s) {
    int len = s.length();
    int count =0;

    for(int i = 0;i<len;i++) {
        count+=isPalindrome(s,i,i);
        count+=isPalindrome(s,i,i+1);
    }
    return count;
}
private int isPalindrome(String s,int left,int right){
    int count=0;
    while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
        count++;left--;right++;
    }
    return count;
}
```
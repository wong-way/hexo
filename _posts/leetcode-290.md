---
title: leetcode-290
date: 2018-04-20 18:23:16
categories: leetcode 
tags:
- oj
- leetcode
description: leetcode第290题-Word Pattern
---
# leetcode-290

## 问题描述

>Given a pattern and a string str, find if str follows the same pattern.
Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in str.

## 问题分析

这道题不同于之前寻找数组中是否存在相同的元素，之前不在同一位置存在不同元素的情况。例如
> 1 - a
> 2 - b
> 1 - c

上述情况之前是不存在的。但是这里存在
>a - cat
>b - dog
>c - dog
如此dog就对应了两个patter。则不满足题意

我们必须保证pattern和字符串中的word是一对一的关系就行了，此处采用hashmap的方式解决。

## 解题代码
```java
public boolean wordPattern(String pattern, String str) {
    String[] token = str.split(" ");
    int len = token.length;
    if (len != pattern.length()) return false;
    Map dict = new HashMap();

    for (int i = 0; i <len ; i++) {
        String word = token[i];
        if (!dict.containsKey(pattern.charAt(i))) {
            if(!dict.containsValue(word))
                dict.put(pattern.charAt(i), word);
            else
                return false;
        } else if (!dict.get( pattern.charAt(i)).equals(word)){
            return false;
        }
    }
    return true;
}
```

## 总结分析
这道题结果运行速度超过96%的提交记录，证明性能较好


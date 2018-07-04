---
title: leetcode_168
date: 2017-07-26 11:56:26
categories: leetcode
tags:
- oj
- leetcode
description: leetcode第168题，Excel Sheet Column Title
---

## leetcode\_Excel Sheet Column Title

>Given a positive integer, return its corresponding column title as appear in an Excel sheet.
>
>For example:
>
>```
>    1 -> A
>    2 -> B
>    3 -> C
>    ...
>    26 -> Z
>    27 -> AA
>    28 -> AB 
>```

1. ## 问题分析**

这道题和之前Excel表格转数字那道题很相似，那是26进制转成10进制，这里是10进制转26进制。思路都是比较相同的。这里采用辗转相除法求出对应的26进制，再用字母表示。

2. ## **解题代码**

```java
public class Solution {
    public String convertToTitle(int n) {
       String res = "";
        while(n != 0) {
            char ch = (char)((n - 1) % 26 + 65);    
            n = (n - 1) / 26;
            res = ch + res;
        }
        return res;
    }
}
```


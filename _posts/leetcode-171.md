---
title: leetcode_171
date: 2017-07-16 23:34:01
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第171道 	Excel Sheet Column Number   
---

> Related to question [Excel Sheet Column Title](https://leetcode.com/problems/excel-sheet-column-title/)
>
> Given a column title as appear in an Excel sheet, return its corresponding column number.
>
> For example:
>
> ```
>     A -> 1
>     B -> 2
>     C -> 3
>     ...
>     Z -> 26
>     AA -> 27
>     AB -> 28 
> ```

这道题就是一个26进制转10进制，只要遍历字符串进行操作就好了

```java
public class Solution {
    public int titleToNumber(String s) {
        int result = 0;
        for(int i =0 ;i< s.length();i++){
            result += (s.charAt(i) - 'A'+1) * Math.pow(26, s.length() - 1 - i);
        }
        return result;
    }
}
```


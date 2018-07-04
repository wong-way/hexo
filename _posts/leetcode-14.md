---
title: leetcode_14
date: 2017-07-04 20:06:53
categories: leetcode
tags:
- leetcode
description: leetcode第14题，Longest Common Prefix
---

### leetcode_14 Longest Common Prefix

> description:Write a function to find the longest common prefix string amongst an array of strings.

这个题目要求我们找出一个字符串数组的最大公共前缀，我先把最短字符串的长度计算出来，采用每个位进行比较的方法，如果满足条件就记录下标，如果不满足就直接跳出循环，减少比较和循环次数

代买如下：

```java
 public static String longestCommonPrefix(String[] strs) {
        String result="";
        if(strs.length == 0)
            return result;
        int index =-1;
        int count = Integer.MAX_VALUE;
        for (String str:strs) {
            if(str.length()<count)
                count=str.length();
        }
        for(int i = 0; i<count ; i++ ){
        	char ch= strs[0].charAt(i);
        	boolean isEqual=true;
        	for (String str :strs) {
        		if(str.charAt(i) != ch)
                {
                    isEqual=false;
                }
        	}
        	if(isEqual)
            {
                index = i;
            }
            else
                break;
        }
        if(index != -1)
            result = strs[0].substring(0,index+1);
        return result;
 }
```

后来使用`Arrays.sort()`,进行排序，找出最短字符串

```java
public static String longestCommonPrefix1(String[] strs) {
        String result = "";
        if (strs.length == 0)
            return result;
        int index = -1;

        Arrays.sort(strs);
        int count = strs[0].length();
        for (int i = 0; i < count; i++) {
            char ch = strs[0].charAt(i);
            boolean isEqual = true;
            for (String str : strs) {
                if (str.charAt(i) != ch) {
                    isEqual = false;
                }
            }
            if (isEqual) {
                index = i;
            } else
                break;
        }
        if (index != -1)
            result = strs[0].substring(0, index + 1);
        return result;
    }
```

最后考虑到`Array.sort()`之后的字符串是已经排序过后的，最大前缀一定在最长字符串和最短字符串之中，于是我们可以将第一个字符串和最后一个字符串拿来作比较，减少循环的次数。

```java
public static String longestCommonPrefix2(String[] strs) {
        String result = "";
        int index = -1;

        if (strs != null && strs.length > 0) {

            Arrays.sort(strs);

            String firstStr = strs[0];
            String lastStr = strs[strs.length - 1];

            for (int i = 0; i < firstStr.length(); i++) {
                if (firstStr.charAt(i) == lastStr.charAt(i)) {
                    index = i;
                } else {
                    return result;
                }
                result = firstStr.substring(0, index + 1);
            }
        }
        return result;
    }
```


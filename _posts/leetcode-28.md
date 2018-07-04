---
title: leectcode_28
date: 2017-07-07 22:08:31
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第28道 implement strStr()
---

### leetcode\_21\_implement strStr()

> Implement strStr().
>
> Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

这道题要求我们寻找一个字符串`substr`是否在另一个字符串`str`中，如果找到则返回在`str`中的下标位置，如果没有找到则返回-1。

我的解决思路如下：

* 首先，如果`substr`为空，则直接返回0；
* 如果，`str`为空，则直接返回-1；
* 排除以上两种情况，就直接进行循环，一个个搜索子串

下面是我的算法代码：

```java
public static int strStr(String haystack, String needle) {
        if (needle.length() == 0)
            return 0;
        if (haystack.length() == 0)
            return -1;
        for (int i = 0; i < haystack.length() + 1; i++) {
            for (int j = 0; j < needle.length() + 1; j++) {

                if (j == needle.length())
                    return i;
                if (i + j == haystack.length())
                    return -1;
                if (haystack.charAt(i + j) != needle.charAt(j))
                    break;
            }
        }
        return 0;
}
```

对于上面代码，可能有人会觉得奇怪，为什么循环终止条件是`haystack.length()+1`,这里解释一下，因为我的`break`的判断条件是放在后面，因此在进行`if(j == needle.length())`时，`j` 已经自加了。因此当`needle`是`haystack`的子串的时候，终止条件是`j= needle.length()`

网站上公认比较好的解题代码如下：

```java
public int strStr(String haystack, String needle) {
    for (int i = 0;; i++) {
        for (int j = 0;; j++) {
            if (j == needle.length())
                return i;
            if (i + j == haystack.length())
                return -1;
            if (needle.charAt(j) != haystack.charAt(i + j))
                break;
        }
    }
}
```

这段代码更加直接，不指定循环终止条件，避免对边界值的检验，，同时采用`if`语句，进行控制。


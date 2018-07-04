---
title: leetcode-455
date: 2018-06-03 15:30:41
categories: leetcode
tags:
- oj
- leetcode
- greedy
description: leetcode第455题，Assign Cookies(贪心算法)
---
# leetcode-455 Assign Cookies

## 问题描述

>Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie. Each child i has a greed factor gi, which is the minimum size of a cookie that the child will be content with; and each cookie j has a size sj. If sj >= gi, we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.
Note:
You may assume the greed factor is always positive. 
You cannot assign more than one cookie to one child.

```text
Example 1:
Input: [1,2,3], [1,1]

Output: 1

Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
Example 2:
Input: [1,2], [1,2,3]

Output: 2

Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 
You have 3 cookies and their sizes are big enough to gratify all of the children, 
You need to output 2.
```

## 解题思路

这道题属于贪心算法范畴，才讲两个数组排序，拟准备采用`count`记录满足孩子的个数，同时遍历饼干数组，如果满足孩子需求就将`count`加一

## 解题代码

```java
public int findContentChildren(int[] g, int[] s) {
    int count = 0;
    Arrays.sort(s);
    Arrays.sort(g);
    for(int i = 0; i<s.length;i++){
        if(count<g.length&&s[i]>=g[count])
            count++;
    }
    return count;
}
```

## 代码改进

上面这种方式只对饼干数组做了限制。这里有一种特殊情况就是当所有孩子的需求都得到满足之后，依然会循环，于是作一小改动如下

```java
public int findContentChildren(int[] g, int[] s) {
    int count = 0;
    Arrays.sort(s);
    Arrays.sort(g);
    for(int i = 0; count<g.length&&i<s.length;i++){
        if(s[i]>=g[count])
            count++;
    }
    return count;
}
```

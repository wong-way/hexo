---
title: leetcode-832
date: 2018-06-03 10:50:31
categories: leetcode
tags:
- oj 
- leetcode
- arrays
description: leetcode 832-Flipping an Image
---
# leetcode-832 Flipping an Image

## 问题描述
>Given a binary matrix A, we want to flip the image horizontally, then invert it, and return the resulting image.
To flip an image horizontally means that each row of the image is reversed.  For example, flipping `[1, 1, 0]` horizontally results in `[0, 1, 1]`.
To invert an image means that each 0 is replaced by 1, and each 1 is replaced by 0. For example, inverting `[0, 1, 1]` results in `[1, 0, 0]`

```text
Example 1:

Input: [[1,1,0],[1,0,1],[0,0,0]]
Output: [[1,0,0],[0,1,0],[1,1,1]]
Explanation: First reverse each row: [[0,1,1],[1,0,1],[0,0,0]].
Then, invert the image: [[1,0,0],[0,1,0],[1,1,1]]
Example 2:

Input: [[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]
Output: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
Explanation: First reverse each row: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]].
Then invert the image: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
```

## 问题分析

这道题就按照他所说的方式，直接对数组进行操作就行了

## 解题代码

```java
public int[][] flipAndInvertImage(int[][] A) {
    int len = A[0].length;
    for (int[] row: A)
        for (int i = 0; i < (len + 1) / 2; ++i) {
            int tmp = row[i] ^ 1;
            row[i] = row[len - 1 - i] ^ 1;
            row[len - 1 - i] = tmp;
        }

    return A;
}
```

另一种交换的方式

```java
int tmp = 1-row[i] ;
row[i] = 1-row[len - 1 - i];
row[len - 1 - i] = tmp;
```

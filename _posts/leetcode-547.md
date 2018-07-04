---
title: leetcode-547
date: 2018-07-04 19:48:52
categories: leetcode
tags:
- oj
- leetcode
- depth first search
description: leetcode No547.Friend Circles
---
# leetcode No547.Friend Circles

## 问题描述

>There are N students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a direct friend of B, and B is a direct friend of C, then A is an indirect friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.
>
>Given a N*N matrix M representing the friend relationship between students in the class. If M[i][j] = 1, then the ith and jth students are direct friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.
>```text
>Example 1:
>Input: 
>[[1,1,0],
> [1,1,0],
> [0,0,1]]
>Output: 2
>Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. 
>The 2nd student himself is in a friend circle. So return 2.
>Example 2:
>Input: 
>[[1,1,0],
> [1,1,1],
> [0,1,1]]
>Output: 1
>Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
>so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.
>```

## 解题思路

遍历每个人`i`的朋友圈，将处在他朋友圈之类`M[i][j]`的人`j`关系中的`M[j][i]`置为0，防止重复计算，同时遍历递归处理`j`的朋友圈。

## 解题代码

```java
    public int findCircleNum(int[][] M) {
        int count = 0;
        for (int i = 0; i < M.length; i++) {
            for (int j = 0; j < M[0].length; j++) {

                if (M[i][j] == 1) {
                    M[i][j] =0;
                    if (i == j) {
                        count++;
                    } else {
                        helper(M, j);
                    }
                }
            }
            System.out.println("count = " + count);
        }
        return count;
    }

    /**
     * 将与people同处一个关系网的置位
     *
     * @param grid 关系网
     * @param i    行数
     */
    private void helper(int[][] grid, int i) {

        for (int j = 0; j < grid[i].length; j++) {
            if (grid[i][j] == 1 && i != j) {

                grid[i][j] = 0;
                grid[i][i] =0;
                helper(grid, j);
            }

        }

    }
```

## 评论区优质代码

这个思路跟我最开始的思路很像。我最开始准备在`findCircleNum`中对每个人进行遍历，然后利用`helper(int people,int[][]M)`中对`people`的朋友圈中的人进行递归处理，我想将遍历过的人进行置位，只是没想到用visited[]数组记录已经遍历的人

```java
public class Solution {
    public void dfs(int[][] M, int[] visited, int i) {
        for (int j = 0; j < M.length; j++) {
            if (M[i][j] == 1 && visited[j] == 0) {
                visited[j] = 1;
                dfs(M, visited, j);
            }
        }
    }
    public int findCircleNum(int[][] M) {
        int[] visited = new int[M.length];
        int count = 0;
        for (int i = 0; i < M.length; i++) {
            if (visited[i] == 0) {
                dfs(M, visited, i);
                count++;
            }
        }
        return count;
    }
}
```
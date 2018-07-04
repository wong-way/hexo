---
title: leetcode-695
date: 2018-07-03 18:19:58
categories: leetcode
tags:
- oj
- leetcode
- depth first search
description: leetcode No695. Max Area of Island
---
# leetcode No695. Max Area of Island

## 问题描述

>Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.
>Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)
>```text
>Example 1:
>[[0,0,1,0,0,0,0,1,0,0,0,0,0],
> [0,0,0,0,0,0,0,1,1,1,0,0,0],
> [0,1,1,0,1,0,0,0,0,0,0,0,0],
> [0,1,0,0,1,1,0,0,1,0,1,0,0],
> [0,1,0,0,1,1,0,0,1,1,1,0,0],
> [0,0,0,0,0,0,0,0,0,0,1,0,0],
> [0,0,0,0,0,0,0,1,1,1,0,0,0],
> [0,0,0,0,0,0,0,1,1,0,0,0,0]]
>Given the above grid, return 6. Note the answer is not 11, because the island must be connected 4-directionally.
>```

## 解题思路

一开我想用dp能不能解决这个问题，后来发现这个问题前后项之间的关系并不大，然后深度优先搜索的问题。因此就递归咯，遇到`1`则将它四周的点进行递归，加上他们的面积，同时为了防止重复的计算，每次将访问过的点置为0。

## 解题代码

```java
    public int maxAreaOfIsland(int[][] grid) {
        int max = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 1) {
                    max = Math.max(max, helper(grid, i, j));
                }
            }
        }
        return max;
    }

    private int helper(int[][] grid, int i, int j) {
        System.out.println("i = " + i + "j= " + j);
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] == 0)
            return 0;
        int area = 1;
        grid[i][j] = 0;
        area += helper(grid, i + 1, j) + helper(grid, i, j + 1) + helper(grid, i - 1, j) + helper(grid, i, j - 1);
        return area;
    }

```

## 评论区代码

一毛一样，没什么好说的。他们的套路已经被我学透了
```java
public int maxAreaOfIsland(int[][] grid) {
    int max_area = 0;
    for(int i = 0; i < grid.length; i++)
        for(int j = 0; j < grid[0].length; j++)
            if(grid[i][j] == 1)max_area = Math.max(max_area, AreaOfIsland(grid, i, j));
    return max_area;
}

public int AreaOfIsland(int[][] grid, int i, int j){
    if( i >= 0 && i < grid.length && j >= 0 && j < grid[0].length && grid[i][j] == 1){
        grid[i][j] = 0;
        return 1 + AreaOfIsland(grid, i+1, j) + AreaOfIsland(grid, i-1, j) + AreaOfIsland(grid, i, j-1) + AreaOfIsland(grid, i, j+1);
    }
    return 0;
}
```
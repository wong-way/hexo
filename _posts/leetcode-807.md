---
title: leetcode-807
date: 2018-06-01 14:37:35
categories: leetcode
tags:
- oj
- leetcode
description: leetcode-807 Max Increase to Keep City Skyline
---
# leetcode-807 Max Increase to Keep City Skyline

##问题描述
>In a 2 dimensional array grid, each value grid[i][j] represents the height of a building located there. We are allowed to increase the height of any number of buildings, by any amount (the amounts can be different for different buildings). Height 0 is considered to be a building as well. 
At the end, the "skyline" when viewed from all four directions of the grid, i.e. top, bottom, left, and right, must be the same as the skyline of the original grid. A city's skyline is the outer contour of the rectangles formed by all the buildings when viewed from a distance. See the following example.
What is the maximum total sum that the height of the buildings can be increased?

```text
Example:
Input: grid = [[3,0,8,4],[2,4,5,7],[9,2,6,3],[0,3,1,0]]
Output: 35
Explanation:
The grid is:
[ [3, 0, 8, 4],
  [2, 4, 5, 7],
  [9, 2, 6, 3],
  [0, 3, 1, 0] ]

The skyline viewed from top or bottom is: [9, 4, 8, 7]
The skyline viewed from left or right is: [8, 7, 9, 3]

The grid after increasing the height of buildings without affecting skylines is:

gridNew = [ [8, 4, 8, 7],
            [7, 4, 7, 7],
            [9, 4, 8, 7],
            [3, 3, 3, 3] ]
```

## 解题思路

这道题和高中数学题有点像，给你一个正视图，侧视图，投影图，让你算出一共有多少积木，而这里是让你保持正视图，侧视图不变的情况下，尽可能提高总高度。
采用下面思路进行解答
* 分别计算出每一行、每一列的最大值
* 遍历整个二维数组，计算出其与`maxRow`、`maxCol`之间较小一个的差值，这个差值即改地点最大可以提高的高度

##解题代码

```java
public int maxIncreaseKeepingSkyline(int[][] grid) {
    int[] vertical=new int[grid[0].length];
    int[] horizontal = new int [grid.length];
    for(int i =0;i<grid.length;i++){
        horizontal[i] = -1;
        for(int j = 0;j<grid[0].length;j++){
            if(grid[i][j]>horizontal[i])
                horizontal[i] = grid[i][j];
            if(grid[j][i]>vertical[i])
                vertical[i] = grid[j][i];
        }
    }
    // System.out.println(Arrays.toString(vertical));
    // System.out.println(Arrays.toString(horizontal));
    int count=0;
    for(int i =0;i<grid.length;i++){
        for(int j = 0;j<grid[0].length;j++){
            count+=Math.min(horizontal[i],vertical[j])-grid[i][j];
        }
    }
    return count;

}
```
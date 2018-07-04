---
title: leetcode-64
date: 2018-04-24 15:31:06
categories: leetcode
tags:
- leetcode
- oj 
- dynamic programming
description: leetcode 第64题，min path sum（动态规划问题）
---
# leetcode 第64题，min path sum（动态规划问题）

## 问题描述
>Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

## 初步尝试

下面的尝试方法，通过了22个测试用例，但是如果遇到`cost[left]==cost[down]`的情况就不能很好的处理
```java
public int minPathSum(int[][] grid) {
    int i = 0;//记录列
    int j = 0;//记录行
    int width = grid[0].length;
    int height = grid.length;
    int right;
    int down;
    int sum = grid[0][0];
    while (i + 1 <= width && j + 1 <= height) {
        if (i + 1 < width)
            right = grid[j][i + 1];
        else
            right = -1;
        if (j + 1 < height)
            down = grid[j + 1][i];
        else
            down = -1;
        if (down == -1 && right == -1)
            break;
        else if (down == -1) {
            System.out.println("go right :" + right);
            sum += right;
            i++;
        } else if (right == -1) {
            System.out.println("go down :" + down);
            sum += down;
            j++;
        } else {//右边，下边都有值，判断往左走还是往右走
            int tempRight = 0;
            int tempDown = 0;
            // 如果往右走,需要对右边节点进行判断是否越界

            if (i + 2 < width)
                tempRight = Math.min(right + grid[j][i + 2], right + grid[j + 1][i + 1]);
            else
                tempRight = right + grid[j + 1][i + 1];

            //如果往下走，只需要检查下边界是否越界
            if (j + 2 < height)
                tempDown = Math.min(down + grid[j + 2][i], down + grid[j + 1][i + 1]);
            else
                tempDown = down + grid[j + 1][i + 1];

            //比较哪个路劲短 
            if (tempDown >= tempRight) {
                System.out.println("go right :" + right);
                sum += right;
                i++;
            } else {
                System.out.println("go down :" + down);
                sum += down;
                j++;
            }
        }
        System.out.println(sum);

    }
    return sum;

}
```

## 再次尝试
由于上述方法似乎并没有用到动态规划的思想，故改之。
此次采用`dp[i][j]= min(dp[i][j-1],dp[i-1][j])`

## 解题代码

```java
 public int minPathSum2(int[][] grid) {
    int i = 0;//记录行
    int j = 0;//记录列
    int width = grid[0].length;
    int height = grid.length;
    int len = width * height;
    int count = 1;
    while (count < len) {
        i = count / width;
        j = count % width;
        if (i - 1 < 0) {
            grid[i][j] = grid[i][j]+grid[i][j - 1];
        } else if (j - 1 < 0) {
            grid[i][j] = grid[i][j]+grid[i - 1][j];
        } else {
            grid[i][j] = grid[i][j]+Math.min(grid[i - 1][j], grid[i][j - 1]);
        }

        count++;
    }

    return grid[height-1][width-1];
}
```

## 再思考

我一开始思考的时候，感觉这个可以用递归程序解决。
`cost[i][j]`表示从第i行，第j列出发到达终点的代价。则`cost[i][j] = min{to[i+i][j]+cost[i+1][j],to[i][j+1]+cost[i][j+1]}`

## 解决方法

```java
public int minPathSum(int[][] grid) {        
        return getMinSum(grid, 0, 0, new int[grid.length][grid[0].length]);
    }
    
    public int getMinSum(int[][] grid, int row, int col, int[][] memo) {
        if(row == grid.length - 1 && col == grid[0].length - 1) return grid[row][col];
        if(row >= grid.length || col >= grid[0].length) return Integer.MAX_VALUE;
        
        if(memo[row][col] <= 0) {
            memo[row][col] = Math.min(getMinSum(grid, row + 1, col, memo), 
                                      getMinSum(grid, row, col + 1, memo)) + grid[row][col];
        }
        return memo[row][col];
    }
```

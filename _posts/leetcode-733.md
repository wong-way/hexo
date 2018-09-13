---
title: leetcode-733
date: 2018-08-02 10:18:30
categories: leetcode
tags: 
- leetcode
- oj
- depth first search
description: leetcode No733. Flood Fill
---
# leetcode No733.Flood Fill

## 问题描述

>n image is represented by a 2-D array of integers, each integer representing the pixel value of the image (from 0 to 65535).
>
>Given a coordinate (sr, sc) representing the starting pixel (row and column) of the flood fill, and a pixel value newColor, "flood fill" the image.
>
>To perform a "flood fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color as the starting pixel), and so on. Replace the color of all of the aforementioned pixels with the newColor.
>
>At the end, return the modified image.
>```text
>Example 1:
>Input: 
>image = [[1,1,1],[1,1,0],[1,0,1]]
>sr = 1, sc = 1, newColor = 2
>Output: [[2,2,2],[2,2,0],[2,0,1]]
>Explanation: 
>From the center of the image (with position (sr, sc) = (1, 1)), all pixels connected 
>by a path of the same color as the starting pixel are colored with the new color.
>Note the bottom corner is not colored 2, because it is not 4-directionally connected
>to the starting pixel.
>```
>Note:
>
>The length of image and image[0] will be in the range [1, 50].
>The given starting pixel will satisfy 0 <= sr < image.length and 0 <= sc < image[0].length.
>The value of each color in image[i][j] and newColor will be an integer in [0, 65535].

## 解题思路

深度优先搜索，如果`[sr,sc]`满足条件，则将其周围的点中满足条件的点进行递归计算

## 解题代码

```java
public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
    if (image[sr][sc] == newColor) return image;
    fill(image,sr,sc,image[sr][sc],newColor);
    return image;
}
private void fill(int[][]image,int sr,int sc,int rawColor,int newColor){
    int rows = image.length;
    int cols = image[0].length;
    if(sr<0||sr>=rows||sc<0||sc>=cols||rawColor!=image[sr][sc]) return ;
    image[sr][sc] = newColor;
    fill(image,sr+1,sc,rawColor,newColor);        
    fill(image,sr-1,sc,rawColor,newColor);
    fill(image,sr,sc+1,rawColor,newColor);
    fill(image,sr,sc-1,rawColor,newColor); 
}
```
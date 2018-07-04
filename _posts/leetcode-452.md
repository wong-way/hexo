---
title: leetcode-452
date: 2018-06-26 22:04:00
categories: leetcode
tags: 
- oj
- leetcode
- greedy
description: leetcode No452. Minimum Number of Arrows to Burst Balloons
---
# leetcode No452. Minimum Number of Arrows to Burst Balloons

## 问题描述

>There are a number of spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter and hence the x-coordinates of start and end of the diameter suffice. Start is always smaller than end. There will be at most 104 balloons.
>
>An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps travelling up infinitely. The problem is to find the minimum number of arrows that must be shot to burst all balloons.
>> ```text
>>Example:
>>
>>Input:
>>[[10,16], [2,8], [1,6], [7,12]]
>>
>>Output:
>>2
>>
>>Explanation:
>>One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons)
>>```

## 解题思路

先将气球的位置按照起点进行排序，采用贪心算法，判断当前气球爆炸时，有多少其他的气球能够爆炸，即利用变量`endPos`记录最后的位置，如果之后的气球的起点在`endPos`之前则可以同时爆炸。

## 解题代码

```java
class Range {
    int low;
    int high;

    public Range(int low, int high) {
        this.low = low;
        this.high = high;
    }

    public Range getIntersection(Range range) {
        if (this.low > range.high || this.high < range.low)
            return null;
        else {
            int lowTmp = low > range.low ? low : range.low;
            int highTmp = high > range.high ? range.high : high;
            return new Range(lowTmp, highTmp);
        }
    }
}

public int findMinArrowShots(int[][] points) {
    if (points.length == 0)
        return 0;
    Arrays.sort(points, Comparator.comparing((int[] arr) -> arr[0]));
    int num = 1;
    int index = 0;//记录当前射爆气球的位置
    Range range = new Range(points[0][0], points[0][1]);
    for (int i = 1; i < points.length; i++) {
        Range now = new Range(points[i][0], points[i][1]);
        Range commen = range.getIntersection(now);
        if (commen == null) {
            num++;
            range = now;
        } else {
            range = commen;
        }
    }
    return num;
}

```

```java
public int findMinArrowShots1(int[][] points) {
    if (points.length == 0)
        return 0;
    Arrays.sort(points, new Comparator<int[]>() {
        @Override
        public int compare(int[] o1, int[] o2) {
            return Integer.compare(o2[0], o1[0]);
        }
    });
    int num = 1;
    int end = points[0][1];//记录当前射爆气球的位置
    for (int i = 1; i < points.length; i++) {
        if (points[i][0] > end) {
            num++;
            end = points[i][1];
        } else {
            end = end > points[i][1] ? points[i][1] : end;
        }
    }
    return num;
}
```
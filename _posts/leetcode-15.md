---
title: leetcode-15
date: 2018-05-23 15:59:37
categories: leetcode
tags:
- leetcode
- oj
description: leetcode第15题3Sum
---
# leetcode-15 3Sum

## 问题描述
>Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.
**Note:**
The solution set must not contain duplicate triplets.
**Example:**

```text
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

## 问题分析
这道题和昨天的4Sum如出一辙。
我准备将数组先排序，然后遍历一遍，对每一个数字从左右两端开始向两头扩散

## 解题代码1

```java
 public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> list = new ArrayList<>();
    for (int i = 1; i < nums.length; i++) {
        int pre = i - 1;
        int back = i + 1;
        while (pre >= 0 && back < nums.length) {
            if (nums[pre] + nums[i] + nums[back] < 0) {
                back++;
            } else if (nums[pre] + nums[i] + nums[back] > 0) {
                pre--;
            } else {
                List<Integer> l = new ArrayList<>();
                l.add(nums[pre]);
                l.add(nums[i]);
                l.add(nums[back]);
                list.add(l);
                back++;
                pre--;
            }
        }

    }

    return (List<List<Integer>>) new ArrayList(new HashSet(list));
}
```
上面这种方法进行了很多重复的计算，例如`-4,-3,-2,0,1`对数字`-2`来说`-2+1<0`，再加上一个小于`-2`的数字是不可能等于0的，但是循环在一开始的时候还是会继续下去

## 解题代码2

```java
 public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> list = new ArrayList<>();
    for (int i = 1; i < nums.length; i++) {
        int pre = i - 1;
        int back = i + 1;
        if(nums[i]>0&&nums[i]+nums[0]>0) continue;
        if(nums[i]<0&&nums[i]+nums[nums.length-1]<0) continue;
        while (pre >= 0 && back < nums.length) {
            if (nums[pre] + nums[i] + nums[back] < 0) {
                back++;
            } else if (nums[pre] + nums[i] + nums[back] > 0) {
                pre--;
            } else {
                List<Integer> l = new ArrayList<>();
                l.add(nums[pre]);
                l.add(nums[i]);
                l.add(nums[back]);
                list.add(l);
                back++;
                pre--;
            }
        }

    }

    return (List<List<Integer>>) new ArrayList(new HashSet(list));
}
```

然而遇到一个毒瘤，测试用例1w个0,然后超时了。从而增加对特殊情况的判断

## 解题代码3

```java
public List<List<Integer>> threeSum3(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> list = new ArrayList<>();
    if (nums[0] > 0)
        return list;

    if (nums[nums.length - 1] < 0)
        return list;

    if (nums[0] == 0 && nums[nums.length - 1] == 0)
    {
        List<Integer> l = new ArrayList<>();
        l.add(0);
        l.add(0);
        l.add(0);
        list.add(l);
        return list;
    }
    for (int i = 1; i < nums.length; i++) {
        int pre = i - 1;
        int back = i + 1;
        if (nums[i] > 0 && nums[i] + nums[0] > 0) continue;
        if (nums[i] < 0 && nums[i] + nums[nums.length - 1] < 0) continue;
        while (pre >= 0 && back < nums.length) {
            if (nums[pre] + nums[i] + nums[back] < 0) {
                back++;
            } else if (nums[pre] + nums[i] + nums[back] > 0) {
                pre--;
            } else {
                List<Integer> l = new ArrayList<>();
                l.add(nums[pre]);
                l.add(nums[i]);
                l.add(nums[back]);
                list.add(l);
                back++;
                pre--;
            }
        }

    }

    return (List<List<Integer>>) new ArrayList(new HashSet(list));
}

```

## 讨论区做法

```java
public List<List<Integer>> threeSum(int[] num) {
    Arrays.sort(num);
    List<List<Integer>> res = new LinkedList<>(); 
    for (int i = 0; i < num.length-2; i++) {
        if (i == 0 || (i > 0 && num[i] != num[i-1])) {
            int lo = i+1, hi = num.length-1, sum = 0 - num[i];
            while (lo < hi) {
                if (num[lo] + num[hi] == sum) {
                    res.add(Arrays.asList(num[i], num[lo], num[hi]));
                    while (lo < hi && num[lo] == num[lo+1]) lo++;
                    while (lo < hi && num[hi] == num[hi-1]) hi--;
                    lo++; hi--;
                } else if (num[lo] + num[hi] < sum) lo++;
                else hi--;
           }
        }
    }
    return res;
}
```
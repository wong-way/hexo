---
title: leetcode_35
date: 2017-07-07 23:17:12
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第35道 Search Insert Position
---

### leetcode\_35\_Search Insert Position

> Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
>
> You may assume no duplicates in the array.
>
> Here are few examples.
> [1,3,5,6], 5 → 2
> [1,3,5,6], 2 → 1
> [1,3,5,6], 7 → 4
> [1,3,5,6], 0 → 0
>

这道题比较简单，直接遍历数组，找到满足`nums[i]>=target`条件的数，直接返回就好，下面是代码：

```java
public int searchInsert(int[] nums, int target) {
        int i;
        for(i = 0; i<nums.length; i++){
            if(nums[i] >= target)
               break;
        }
        return i;
    }
```


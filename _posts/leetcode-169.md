---
title: leetcode_169
date: 2017-07-26 14:35:52
categories: leetcode
tags:
- oj
- leetcode
description: leetcode第169题，Majority Element
---

## leetcode\_169\_Majority Element

>Given an array of size *n*, find the majority element. The majority element is the element that appears **more than** `⌊ n/2 ⌋` times.
>
>You may assume that the array is non-empty and the majority element always exist in the array.

1. ## **问题分析**

这个`majority`是指在数组中出现次数超过一半的数字，这意味着其他数字加起来的数量都没有它多，那么利用一个`count`来计数，如果和预设的`major`相同，则让`count`加一，如果不同则减一，当`count`降为0时，则说明次数被抵消掉，那么更新`major`，由于`major`次数是最多的，所以遍历结束之后，`major`中的值一定是所求数字。

2. ## **解题代码**

```java
public class Solution {
    public int majorityElement(int[] nums) {
        int major = nums[0];
        int count = 1;
        for(int i = 1; i<nums.length ; i++){
			if(nums[i] == major)
				count ++;
			else if(count == 0)
			{
				major = nums[i];
				count ++;
			}else count --;
        }
    }
}
```


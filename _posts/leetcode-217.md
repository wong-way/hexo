---
title: leetcode-217
date: 2018-04-13 22:02:08
tags: 
  - leetcode     
  - oj
categories: leetcode
---
# leetcode-217

## 问题描述

>Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct. 

## 问题分析

* 这道题让我们判断数组中是否存在重复的元素，如果不考虑性能的话，时间复杂度为O(n^2)很容易就解决
* 进行两次循环，外层循环遍历整个数组，内层循环寻找是否存在相同的元素，如果存在直接返回
* 利用hash，每次的查找时间为O(1),但是这里面存在负数，于是采用java的hashmap

## 解题代码

```java
public boolean containsDuplicate(int[] nums) {
    Map<Integer, Integer> map = new HashMap<Integer,Integer>();
    for(int i =0;i<nums.length;i++) {
        if(map.containsKey(nums[i]))
            return  true;
        map.put(nums[i], 1);
    }
    return false;
}
```

## 总结问题

* 果不其然，采用两层循环当数据量过大的时候直接超时
* 采用hashmap的方式能够成功解决这个问题，但是只是超过了17%的解答

## 算法优化

* 在评论区有人先将数组排序，再选择是否有相邻的两个元素相同，当时我也想到用这个方法，只是我当时智障的想着排序时间复杂度也是O(n^2)，没想到快排等。而他们直接用`Arrays.sort()`

```java
public boolean containsDuplicate(int[] nums) {
    Arrays.sort(nums);
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] == nums[i - 1])
            return true;
    }
    return false;
}
```

* 评论区也有人使用hash,只是我考虑到数字有正有负，不好直接做数组的索引。之前做过一次相似的寻找是否有重复的字母，我是采用一个26位的数组来存，`array[A-'0'] = 1` 这种方式。每次寻找是否重复的时候时间复杂度为O(1)。这里也可以采用相似的方式处理。

```java
public boolean containsDuplicate(int[] nums) {
    if(nums == null || nums.length == 1) return false;
    // 找出最大最小值
    // 为什么要找出最大最小值，是为了保证每个值都有一个对应的数组空间
    // boolean[] boo = new boolean[nums.length]会使用更多的空间
    int max = Integer.MIN_VALUE;
    int min = Integer.MAX_VALUE;
    for(int num : nums){
        if(num > max)
            max = num;
        if(num < min)
            min = num;
    }
    // 利用hash表，检查是否已经存在
    boolean[] bool = new boolean[max - min + 1];
    for(int num : nums){
        if(bool[num - min])
            return true;
        else
            bool[num - min] = true;
    }
    
    return false;
    
    }
```
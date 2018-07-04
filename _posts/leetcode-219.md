---
title: leetcode-219
date: 2018-04-14 11:21:45
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第219道
---
# leetcode-219 Contains Duplicate II

## 问题描述

>Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that `nums[i] = nums[j]` and the absolute difference between i and j is at most k.

## 问题分析

* 这道题由于涉及到位置的问题，那么就不能再把数组先做一个排序，然后再处理的方式
* 这道题就和上一次没什么太大区别，Contains Duplicate I是采用boolea类型的数组来存他是否存在，那么我们这里采用整型数组来记录他在数组中的位置

## 解题代码

```java
 public boolean containsNearbyDuplicate(int[] nums, int k) {
    if(k>nums.length||nums.length==0)
        return false;

    int max = Integer.MIN_VALUE;
    int min = Integer.MAX_VALUE;
    for(int num : nums){
        if(num > max)
            max = num;
        if(num <= min)
            min = num;
    }
    long[] arr = new long[max-min+1];
    // value：
    // 0-表示没有元素
    // 1-表示在nums中的下标为0，是第一个元素
    // 为了解决下标为0的索引问题
    // 例如 nums[10]=100,nums[0]=100,当处理nums[10]的时候，发现arr[100] = 0 ,此时就会发生歧义，不知是否已经初始化还是本身下标就为0
    for (int i = 0; i < nums.length; i++) {
        long len = 0;
        int num = nums[i];
        System.out.println(num);
        System.out.println(min);
        System.out.println(num -min);
        if (arr[nums[i]-min] != 0) {
            len = Math.abs(arr[nums[i]-min] - i - 1);
            if(len == k)
                return true;
//                else//存在多个重复的情况，进行多次检验
//                    arr[nums[i]]=i+1;
        }
        else{
            arr[nums[i]-min]=i+1;
        }
    }
    return false;
}
```

## 分析总结

心中的野马奔腾而过，测试用例:`[2147483647,-2147483648,2147483647,-2147483648]`虽然只有四个数字，但是将要创建一个2147483647+2147483648的数组，空间成本太大，同时还超过int型的范围，于是我还是改用map类型来做

```java
public boolean containsNearbyDuplicate(int[] nums, int k) {

    Map<Integer, Integer> map = new HashMap<Integer, Integer>();
    int len ;
    for (int i = 0; i < nums.length; i++) {

        int num = nums[i];
        if (map.get(num) != null) {
            System.out.println(map.get(num));
            len =i- map.get(num);
            if (len <= k)
                return true;
        } else
            map.put(num, i);
    }
    return false;
}
```
做完发现，***，我题意理解不到位，对这道题没有理解题意，下面给出评论区方法
```java
public boolean containsNearbyDuplicate(int[] nums, int k) {
    Set<Integer> set = new HashSet<Integer>();
    for(int i = 0; i < nums.length; i++){
        if(i > k) set.remove(nums[i-k-1]); //remove element if its distance to nums[i] is not lesser than k
        if(!set.add(nums[i])) return true; //because all still existed elements is closer than k distance to the num[i], therefore if the add() return false, it means there's a same value element already existed within the distance k, therefore return true.
    }
    return false;
}
```

## 问题重新梳理

>Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that `nums[i] = nums[j]` and the absolute difference between i and j is at most k.
* 这里所说的 `at mosk k`其实是`i`和`j`距离不超过，是`less than`的意思

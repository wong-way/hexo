---
title: leetcode-561
date: 2018-08-02 10:18:23
categories: leetcode
tags: 
- bucket sort
- oj
- array
- leetcode
description: leetcode No561. Array Partition I
---
# leetcode No561. Array Partition I

## 问题描述
>Given an array of 2n integers, your task is to group these integers into n pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.
>```text
>Example 1:
>Input: [1,4,3,2]
>
>Output: 4
>Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).
>```
>**Note:**
>n is a positive integer, which is in the range of [1, 10000].
>All the integers in the array will be in the range of [-10000, 10000].

## 解题思路
这道题要求是求max{min{a1,b1}+min{a2,b2}+min{a3,b3}+min{a4,b4}...min{an,bn}},如果我这里每一个ai<bi，那么result = a1+a2+a3...+am;这里要保证每一个a均有一个b比它大，因此我只需要将数组排序之后取奇数位就可以了。

## 解题代码

```java
public int arrayPairSum(int[] nums) {
    int sum=0;
    Arrays.sort(nums);
    for(int i=0;i<nums.length;i++){
        if(i%2==0)
            sum+=nums[i];
    }
    return sum;
}
```

```java
public int arrayPairSum(int[] nums) {
    int sum=0;
    int[] bucket = new int[20001];
    for(int i=0;i<nums.length;i++){
        bucket[nums[i]+10000]++;
    }
    int count = 0;
    for(int j =0;j<bucket.length;j++){
        while(bucket[j]>0)
        {
            if(count%2==0)
                sum+=(j-10000);
            count++;
            bucket[j]--;
        }   
    }
    return sum;
}
```

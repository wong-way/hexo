---
title: leetcode-377
date: 2018-09-07 16:24:50
categories: leetcode
tags: 
- leetcode
- oj
- binary tree
description: leetcode No377.Combination Sum IV(DP)
---
# leetcode No377.Combination Sum IV

## 问题描述
>Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.
>Example:
>```text
>nums = [1, 2, 3]
>target = 4
>
>The possible combination ways are:
>(1, 1, 1, 1)
>(1, 1, 2)
>(1, 2, 1)
>(1, 3)
>(2, 1, 1)
>(2, 2)
>(3, 1)
>Note that different sequences are counted as different combinations.
>Therefore the output is 7.
>```text

## 解题思路

### 解题思路一

第一个想到的思路是递归，`combinationSum4(int []num,int target)`是求出`num`数组中和等于`target`的排列组合的数量，那么我对`num`进行遍历，递归计算`combinationSum4(num,targer-num[i])`即可

### 解题代码一

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int count = 0;
        if(target == 0) return 1;
        else if(target<0) return 0;
        else{
            for(int i = 0 ;i<nums.length;i++){
                count+= combinationSum4(nums,target-nums[i]);
            }
        }
        return count;
    }
}
```

### 解题思路二

上面这种方式可以解决问题，但是重复进行递归的次数太多，时间复杂度很高，因此采取备忘录算法，将计算过的`target`的排练组合次数存起来，不用反复进行计算

### 解题代码二

```java
class Solution {
    Map<Integer,Integer> map = new HashMap<>();
    public int combinationSum4(int[] nums, int target) {
        int count = 0;
        if(target == 0) return 1;
        else if(target<0) return 0;
        else{
            for(int i = 0 ;i<nums.length;i++){
                int temp;
                if(map.containsKey(target-nums[i])){
                    temp = map.get(target-nums[i]);
                }else{
                    temp = combinationSum4(nums,target-nums[i]);
                    map.put(target-nums[i],temp);
                }
                count+=temp;
            }
        }
        return count;
    }
}
```

### 解题思路三

这个思路来自评论区，上诉方式是自顶向下的方式，那么采用动态规划自底向上方法解决这个问题

### 解题代码三

```java
public int combinationSum4(int[] nums, int target) {
    int[] comb = new int[target + 1];
    comb[0] = 1;
    for (int i = 1; i < comb.length; i++) {
        for (int j = 0; j < nums.length; j++) {
            if (i - nums[j] >= 0) {
                comb[i] += comb[i - nums[j]];
            }
        }
    }
    return comb[target];
}
```
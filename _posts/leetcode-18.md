---
title: leetcode-18
date: 2018-05-22 23:09:48
categories: leetcode
tags:
- oj 
- leetcode
- two pointer
description: leetcode第18题，4Sum
---
# leetcode-18 4Sum

## 问题描述

>Given an array nums of n integers and an integer target, are there elements a, b, c, and d in nums such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.
**Note:**
The solution set must not contain duplicate quadruplets.
**Example:**
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

```text
A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

## 问题分析

* 当我看到这道题的时候，结合我之前做的`3Sum closet`一题我是用O(n^2)的方法解决的。所以我感觉利用相似的方式，先遍历整个数组，然后求`3Sum(nums,targer-num)`,那么这道题时间复杂度为O(n^3)肯定能做出来，于是我想直接挑战O(n^2)
* 我看到这道题归类于`two pointer`一类，我想利用遍历一遍数组,然后计算2Sum(nums,target-num1-num2)的方式来解决，此时时间复杂度为O(n^2)

## 解题代码

下面的代码有不太完善的地方，leetcode的测试用例只通过了231/282

```java
public List<List<Integer>> fourSum(int[] nums, int target) {
    List<List<Integer>> result = new ArrayList<>();
    int i = 0;
    int j = nums.length - 1;

    while (i < j-1) {
        System.out.println("begin"+i+" "+j+" "+nums[i]+" "+nums[j]);
        List<List<Integer>> t1 = twoSum(nums, i, j, target - nums[i] - nums[j]);
        for (List l : t1) {
            l.add(nums[i]);
            l.add(nums[j]);
            result.add(l);
        }
        System.out.println("begin"+i+" "+(j-1)+" "+nums[i]+" "+nums[j-1]);
        List<List<Integer>> t2 = twoSum(nums, i, j-1, target - nums[i] - nums[j-1]);
        for (List l : t2) {
            l.add(nums[i]);
            l.add(nums[j-1]);
            result.add(l);
        }
        System.out.println("begin"+(i+1)+" "+j+" "+nums[i+1]+" "+nums[j]);
        List<List<Integer>> t3 = twoSum(nums, i+1, j, target - nums[i+1] - nums[j]);
        for (List l : t3) {
            l.add(nums[i+1]);
            l.add(nums[j]);
            result.add(l);
        }
        i++;
        j--;
    }
    for (List l : result) {
        Collections.sort(l);
    }
    HashSet h = new HashSet(result);
    result.clear();
    result.addAll(h);
    return result;

}

public List<List<Integer>> twoSum(int[] nums, int exceptOne, int exceptTwo, int target) {
    System.out.println("target - "+target);
    int[] arr = new int[nums.length - 2];
    int index =0;
    for (int i = 0; i <nums.length; i++) {
        if (i != exceptOne && i != exceptTwo) {
            arr[index++] = nums[i];
        }
    }
    Arrays.sort(arr);
    List<List<Integer>> twoSumList = new ArrayList<>();
    int i = 0;
    int j = arr.length - 1;
    while (i < j) {
//            if (arr[i] + arr[j-1] == target) {
//                List<Integer> temp = new ArrayList<>();
//                temp.add(arr[i]);
//                temp.add(arr[j-1]);
//                twoSumList.add(temp);
//            }
//            if (arr[i+1] + arr[j] == target) {
//                List<Integer> temp = new ArrayList<>();
//                temp.add(arr[i+1]);
//                temp.add(arr[j]);
//                twoSumList.add(temp);
//            }
        if (arr[i] + arr[j] == target) {
            List<Integer> temp = new ArrayList<>();
            temp.add(arr[i]);
            temp.add(arr[j]);
            twoSumList.add(temp);
            i++;
            j--;
        }else if(arr[i] + arr[j] < target){
            i++;
        }else{
            j--;
        }
    }
    for (List l : twoSumList) {
        for (Object x : l) {
            System.out.print((int)x+" ");
        }
        System.out.println();
    }

    return twoSumList;
}
```

## 完整解决方法

待续
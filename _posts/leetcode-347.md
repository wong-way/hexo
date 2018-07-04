---
title: leetcode-347
date: 2018-06-24 12:48:17
categories: leetcode
tags:
- oj
- leetcode
- heap
- bucket sort
description: leetcode No347. Top K Frequent Elements(bucket sort)
---
# leetcode No347. Top K Frequent Elements

## 问题描述

>Given a non-empty array of integers, return the k most frequent elements.
>```text
>For example,
>Given [1,1,1,2,2,3] and k = 2, return [1,2].
>```

## 问题分析

这道题我得思路是，首先统计每个数字出现的频率，得到`key-value`这样的键值对，然后将`value`排序，从高到低获取`key`，中间要用到键值反转，同时这里可以使用桶排序来解决这个问题

## 解题代码

```java
public List<Integer> topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> keyMap = new HashMap<Integer, Integer>();
    List<Integer>[] bucket = new List[nums.length + 1];
    for (int n : nums) {
        keyMap.put(n, keyMap.getOrDefault(n, 0) + 1);
    }
    for (int key : keyMap.keySet()) {
        int frequency = keyMap.get(key);
        if (bucket[frequency] == null) {
            bucket[frequency] = new ArrayList<>();
        }
        bucket[frequency].add(key);
    }

    List<Integer> res = new ArrayList<>();

    for(int pos = bucket.length-1; pos >= 0; pos--){
        if(bucket[pos] != null){
            for(int i = 0; i < bucket[pos].size() && res.size() < k; i++)
                res.add(bucket[pos].get(i));
        }
    }
    return res;

}
```
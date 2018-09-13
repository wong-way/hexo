---
title: leetcode-349
date: 2018-08-02 10:18:14
categories: leetcode
tags: 
- hash table
- oj
- two pointer
- binary search
- leetcode
description: leetcode No349. Intersection of Two Arrays
---
# leetcode No349. Intersection of Two Arrays

## 问题描述
>Given two arrays, write a function to compute their intersection.
>
>Example:
>Given nums1 = [1, 2, 2, 1], nums2 = [2, 2], return [2].

## 解题思路
1. 第一种方式对每一个在数组一中的元素，我都在数组二中去寻找他是否存在，时间复杂度O(n^2)
2. 第二种方式先将数组排序，然后使用两个指针，指向两个数组当前位置的数，如果相等则添加到相应的数组中；如果不相等则移动对应的指针。
3. 第三种方式，利用hash表的特性，使用一个hashtable先记录数组一中存在哪些元素，然后对数组二中的每一个元素判断在hashtable中是否存在;
## 解题代码

```java
public int[] intersection(int[] nums1, int[] nums2) {
       
    List<Integer> list = new ArrayList<>();
    int index1 = 0;
    int index2 = 0;
    Arrays.sort(nums1);
    Arrays.sort(nums2);
    
    while (index1 < nums1.length && index2 < nums2.length) {

        if (nums1[index1] == nums2[index2]) {
            if (!list.contains(nums1[index1]))
                list.add(nums1[index1]);
            index1++;
            index2++;
        } else if (nums1[index1] < nums2[index2]) {
            index1++;
        } else {
            index2++;
        }
    }

    int[] d = new int[list.size()];
    for (int i = 0; i < list.size(); i++) {
        d[i] = list.get(i);
    }
    return d;
}

```

```java
public int[] intersection(int[] nums1, int[] nums2) {
    Set<Integer> set = new HashSet<>();
    Set<Integer> intersect = new HashSet<>();
    for (int i = 0; i < nums1.length; i++) {
        set.add(nums1[i]);
    }
    for (int i = 0; i < nums2.length; i++) {
        if (set.contains(nums2[i])) {
            intersect.add(nums2[i]);
        }
    }
    int[] result = new int[intersect.size()];
    int i = 0;
    for (Integer num : intersect) {
        result[i++] = num;
    }
    return result;
}
```
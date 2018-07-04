---
title: leetcode_88
date: 2017-07-12 22:48:16
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第88道 Merge Sorted Array
---

### leetcode\_88\_Merge Sorted Array

> Given two sorted integer arrays *nums1* and *nums2*, merge *nums2* into *nums1* as one sorted array.

> **Note:**
>
> You may assume that *nums1* has enough space (size that is greater or equal to *m* + *n*) to hold additional elements from *nums2*. The number of elements initialized in *nums1* and *nums2* are *m* and *n* respectively.

**问题简析**

其实个人感觉这道题和排序算法比较像，只要把两个已经排序的数组合并成一个排好序的数组就可以了。

**解题思路**

* 方法一，将两个数组连接起来，然后再排序，而且JAVA排序还很方便，调用`Arrays.sort`即可。
* 方法二，一边连接一边排序。整体思想和选择排序相似，将每一个数字放在它应该在的位置，最大的放在最后一个，最小的放在最前面。

**解题代码**

```java
 public void merge(int[] nums1, int m, int[] nums2, int n) {
        m = m - 1;
        n = n - 1;
        for(int i=m+n+1;i>=0;i--){
            if(m<0)
                nums1[i]=nums2[n--];
            else if(n<0)
                nums1[i]=nums1[m--];
            else {
                nums1[i] = nums2[n] > nums1[m] ? nums2[n--] : nums1[m--];
            }
        }
 }
```

**示例代码**

```java
public void merge(int A[], int m, int B[], int n) {
    int i=m-1, j=n-1, k=m+n-1;
    while (i>-1 && j>-1) A[k--]= (A[i]>B[j]) ? A[i--] : B[j--];
    while (j>-1)         A[k--]=B[j--];
}
```

这三行代码真的很精髓，我一开始看的时候，觉得最后只对B数组剩下的元素进行处理，但是A数组剩下的却没有进行相应的处理，我觉得这里可能会出现问题。但是我后来反应过来，A数组如果出现剩下的元素，那么它本身所在的位置就是它最终应该出现的位置！！不得不佩服写出这个算法的人，很厉害。
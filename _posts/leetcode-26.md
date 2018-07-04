---
title: leetcode_26
date: 2017-07-06 20:08:19
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第26道 removeDuplicates
---

### leetcode\_26\_removeDuplicates

> Given a sorted array, remove the duplicates in place such that each element appear only *once* and return the new length.
>
> Do not allocate extra space for another array, you must do this in place with constant memory.
>
> For example,
> Given input array *nums* = `[1,1,2]`,
>
> Your function should return length = `2`, with the first two elements of *nums* being `1` and `2` respectively. It doesn't matter what you leave beyond the new length.

一开始，我看到函数的返回值的时候，我以为只是统计不重复元素的数量，同时题目要求我们不能使用额外的空间。我注意到该数组是已经排好序的数组，因此我采取以下方法

1. **将计数器`count`初始化为1**
2. **利用循环，判断`nums[i]`与`nums[i-1]`的值是否相同，如果相同，继续执行循环，如果不同，就将计数器加一**

代码如下：

```java
public static int removeDuplicates(int[] nums) {
  if(nums.length==0)
    return 0;
  int count = 1;
  for (int i = 1; i < nums.length; i++) {
    if (nums[i] == nums[i - 1])
      continue;
    else      
      count++;
  }
  return count;
}
```

但是放到leetcode上面去提交，发现我对题意的理解有问题，题目要求

> Your function should return length = `2`, with the first two elements of *nums* being `1` and `2` respectively

不光是要返回个数n，同时要求前n个元素就是不重复的那些元素。

因为在我的计数器count，不仅仅是非重复的元素个数，而且`满足count<i`同时可以在下一次计数器更新的时候，用来指定下一个非重复元素应该放置的位置，即`num[count] =num[i]`，因此我做出以下改进：

1. **将计数器`count`初始化为1**
2. **利用循环，判断`nums[i]`与`nums[i-1]`的值是否相同，**
   * **如果相同，继续执行循环**。
   * **如果不同，就将计数器加一，同时更新num[count]**

代码如下

```java
public static int removeDuplicates(int[] nums) {
        if(nums.length==0)
            return 0;
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i - 1])
                continue;
            else{
                nums[count]=nums[i];
                count++;
            }
        }
        return count;
}
```

最后再次做出一个小调整

```java
public static int removeDuplicates(int[] nums) {
        if(nums.length==0)
            return 0;
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] != nums[i - 1]){
                nums[count++]=nums[i];
            }
        }
        return count;
}
```


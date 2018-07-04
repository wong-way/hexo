---
title: leetcode-202
date: 2018-04-10 19:46:34
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第202道 Happy Number
---

# 快乐数

### 问题描述

>写一个算法来判断一个数是不是“快乐数”。
>
>一个数是不是快乐是这么定义的：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，或是无限循环但始终变不到 1。如果可以变为 1，那么这个数就是快乐数。
>
>**案例: **19 是一个快乐数。
>$$
>1^2+9^2 =82
>$$
>
>$$
>2^2+8^2=68
>$$
>
>$$
>6^2+8^2=100
>$$
>
>$$
>1^2+0^2+0^2=1
>$$
>



### 问题分析

* 首先，如果一个数是10的倍数，则一定是快乐数，同时1一定也是快乐数
* 如果不是快乐数，那么是需要无限循环都不等于1。
  * 对于无限循环来说无法量化，则在计算过程中验证这个数字是否在之前计算过程中出现过，如果出现过则会陷入循环，则一定不是快乐数。 
  * 否则，继续进行循环
* 如果不是循环数，则在此过程中出现的数字一定会呈现一种**环状结构**

### 解题代码

```java
public static boolean isHappy(int n) {
    List<Integer> list = new ArrayList<Integer>();
    while (n != 1) {
        int temp = n;
        int next = 0;
        while (temp != 0) {
            next += Math.pow(temp % 10, 2);
            temp /= 10;
        }
        if (list.contains(next)) {
            return false;
        } else {
            n = next;
            list.add(next);
        }
    }
    return true;

}
```

### 分析总结

使用我的方法运行leetcode的测试用例，运行速度比72%的提交记录快，但是仔细一想，这种方式的空间开销比较大，如果循环次数过多，那么list占用的空间则会增大。

### 社区代码

不言而喻，相当简洁直观

```java
public int next(int n) {
    int result = 0;
    while (n > 0) {
        int remainder = n % 10;
        result += remainder * remainder;
        n /= 10;
    }
    return result;
}

public boolean isHappy(int n) {
    int slow = n;
    int fast = n;
    do {
        slow = next(slow);
        fast = next(next(fast));
        if (slow == 1 || fast == 1) {
            return true;
        }
    } while (slow != fast);
    return false;
}
```


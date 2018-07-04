---
title: leetcode_66
date: 2017-07-10 09:10:08
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第66道 Plus One
---

### leetcode\_66\_Plus One

> Given a non-negative integer represented as a **non-empty** array of digits, plus one to the integer.
>
> You may assume the integer do not contain any leading zero, except the number 0 itself.
>
> The digits are stored such that the most significant digit is at the head of the list.

**题目分析**

这道题最重要的就是考虑进位，以及在申明数组的时候，数组大小的确定。

**解题思路**

* 申明一个同`nums[]`同样大小的数组,命名为`result[]`。申明一个整型变量，命名为`carry`。
* 从数字的最低位开始运算，最低位加一。`result[i] = (nums[i]+carry)%10`，`carry = (nums[i]+carry)/10;
* 注意，如果循环结束之后，`carry = 1`，则说明，最后需要进位。那么则需要重新申明一个数组，大小为`nums.length+1`。首位置为1

实验代码

```java
public static int[] plusOne1(int[] digits) {
        int[] result = new int[digits.length];
        int carry = 0;
        result[digits.length-1]=(digits[digits.length-1]+1)%10;
        carry=(digits[digits.length-1]+1)/10;
        for(int i= digits.length-2; i>=0;i--){

            result[i]+=(digits[i])%10+carry;
            carry=(digits[i])/10;
        }
        if(carry == 1){
        	int[] result1 = new int[digits.length+1];
        	result1[0]=1;
        	for(int i = 0;i<digits.length;i++){
        		result1[i+1]=result[i];
        	}
        	return result1;
        }
        return result;

    }
```

**一开始的错误代码**

一开始我直接申明一个大小为`nums.length+1`的数组。然后首位置为0，在需要的时候在进行置位。但提交的时候并不能通过。但是也可以作为一种解决思路，有可能对其他问题有所启示。

```java
public static int[] plusOne(int[] digits) {
        int[] result = new int[digits.length+1];
        int carry = 0;
        result[digits.length]=(digits[digits.length-1]+1)%10;
        carry=(digits[digits.length-1]+1)/10;
        for(int i= digits.length-2; i>=0;i--){

            result[i+1]+=(digits[i])%10+carry;
            carry=(digits[i])/10;
        }
        return result;

    }
```


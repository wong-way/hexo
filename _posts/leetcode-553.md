---
title: leetcode-553
date: 2018-07-02 12:46:06
categories: leetcode
tags:
- oj
- leetcode
- String
description: leetcodeNo.553 Optimal Division
---
# leetcodeNo.553 Optimal Division

## 问题描述

>Given a list of positive integers, the adjacent integers will perform the float division. For example, [2,3,4] -> 2 / 3 / 4.
However, you can add any number of parenthesis at any position to change the priority of operations. You should find out how to add parenthesis to get the maximum result, and return the corresponding expression in string format. Your expression should NOT contain redundant parenthesis.
>```text
>Example:
>Input: [1000,100,10,2]
>Output: "1000/(100/10/2)"
>Explanation:
>1000/(100/10/2) = 1000/((100/10)/2) = 200
>However, the bold parenthesis in "1000/((100/10)/2)" are redundant, 
>since they don't influence the operation priority. So you should return "1000/(100/10/2)". 
>
>Other cases:
>1000/(100/10)/2 = 50
>1000/(100/(10/2)) = 50
>1000/100/10/2 = 0.5
>1000/100/(10/2) = 2
>```

## 解题思路

通过不断的构造括号，我们可以改变除数和被除数。首先，`nums[0]`肯定是被除数的一部分，`nums[1]`肯定是除数的一部分这两个没办法通过加括号来改变。我们要得到结果的最大值，则需要将被除数最大化，除数最小化，得到一个`(X*Z/Y*W)`形式的字符串。因此去`W=1`,除数为`nums[1]`,`Z=nums[2]*nums[3]...nums[n-1]`即为最终结果

## 解题代码

```java
public String optimalDivision(int[] nums) {
    String ret = "";
    if (nums.length == 0) {
        return ret;
    }
    if (nums.length == 1) {
        ret = Integer.toString(nums[0]);
        return ret;
    }
    if (nums.length == 2) {
        ret = Integer.toString(nums[0]) + "/" + Integer.toString(nums[1]);
        return ret;
    }

    ret = Integer.toString(nums[0]) + "/(" + Integer.toString(nums[1]);
    for (int i=2; i<nums.length; i++) {
        ret += "/" + Integer.toString(nums[i]);
    }
    ret += ")";
    return ret;
}

```
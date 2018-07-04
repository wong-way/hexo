---
title: leetcode_53
date: 2017-07-09 23:14:10
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第53道 Maximum Subarray
---

### leetcode\_53\_Maximum Subarray

> Find the contiguous subarray within an array (containing at least one number) which has the largest sum.
>
> For example, given the array `[-2,1,-3,4,-1,2,1,-5,4]`,
>
> the contiguous subarray `[4,-1,2,1]` has the largest sum = `6`.
>
> If you have figured out the O(*n*) solution, try coding another solution using the divide and conquer approach, which is more subtle.

**题目分析**

这道题要求我们找出一个数组中的最大子数组的和，一开始我看到这道题目，我记得以前我是看到过这道题的，当时我是用了三个循环把它跑出来，具体是怎么我已经不记得了。但是我也不是当年那个青涩的我了，我现在看着它我就想用一个循环把它弄出来，时间复杂度达到O(n)。

**解题思路**

* 申明两个变量`max`和`temp`，
* 循环，使`temp`每次都加上数组的值
  * 如果，运算后`temp>max`，那么更新`max`
  * 如果，运算后`temp<0`，则令`temp=0`。因为在此之前的一串数字已经不具有价值。为什么说他不具有价值，这是因为前面的结果的和已经是个负数，那么我再把它与后面的数字相加的时候，他只会造成负增长，所以我们令`temp = 0`重新开始，寻找下一个潜在的最大值。

**解题代码**

```java
public class Solution53 {
    public static int maxSubArray(int[] nums) {
        int max = nums[0];
        int temp = 0;
        for(int i= 0 ; i< nums.length ; i++){
            temp += nums[i];
            if(temp>max)
            {
                max = temp;
            }
            if(temp<0){
                temp = 0;
            }
        }
        return max;
    }
    public static void main(String[] args) {
        int[] test = {-2,1,-3,4,-1,2,1,-5,4};
        int result = maxSubArray(test);
        System.out.println(result);

    }
}
```

一开始我是没什么思路的，因为我一边看直播，一边想这个问题。最后想着想着就去看直播去了，做什么题哦。然后LPL打赢了，夺冠了。我再回来解决它。总之今天的任务完成了！
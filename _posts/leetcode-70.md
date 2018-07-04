---
title: leetcode_70
date: 2017-07-11 23:57:41
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第70道 Climbing Stairs
---

### leetcode\_70\_Climbing Stairs

>You are climbing a stair case. It takes *n* steps to reach to the top.
>
>Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

经典的爬楼梯问题

一开始的递归做法

```java
public static int climbStairs(int n) {
        if(n == 0)
            return 0;
        else if(n == 1)
            return 1;
        else if(n == 2)
            return 2;
        return climbStairs(n-1)+climbStairs(n-2);
}
```

做了之后，发现当n=44时，时间溢出了。只能转用循环。

```java
public int climbStairs(int n) {
    	if(n == 0)
    		return 0;
        else if(n == 1)
        	return 1;
        int [] result = new int[n];
        result[0] =1;
        result[1] = 2;
        for(int i=2;i<n;i++){
        	result [i] = result[i-1]+result[i-2];
        }
        return result[n-1];
}

```


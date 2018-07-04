---
title: leetcode_118_119
date: 2017-07-16 22:56:50
tags: 
- leetcode
- oj
categories: leetcode
description: leetcode算法题第118 Pascal's Triangle、119道Pascal's Triangle II
---

### leetcode\_118\_Pascal's Triangle

> Given *numRows*, generate the first *numRows* of Pascal's triangle.
>
> For example, given *numRows* = 5,
> Return
>
> ```
> [
>      [1],
>     [1,1],
>    [1,2,1],
>   [1,3,3,1],
>  [1,4,6,4,1]
> ]
> ```

输出杨辉三角，只要知道杨辉三角是怎么构建就能够很容易的把它实现.直接贴代码

```java
public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> mList = new ArrayList<List<Integer>>();
        for(int i = 0; i< numRows;i++){
        	List<Integer> intList = new ArrayList<Integer>();
			for(int j=0;j<i;j++){
				if(j == 0|| j==i)
					intList.add(1);
				else{
					intList.add(mList.get(i-1).get(j)+mList.get(i-1).get(j-1));
				}
			}
        	mList.add(intList);
        }
        return mList;
}
```

### leetcode\_119\_Pascal's Triangle II

> Given an index *k*, return the *k*th row of the Pascal's triangle.
>
> For example, given *k* = 3,
>
> Return `[1,3,3,1]`.

给出`rowIndex`，输出杨辉三角对应的一层，直接在上一题的基础上返回`mList.get(rowIndex)`

```java
public List<Integer> getRow(int rowIndex) {
       List<List<Integer>> mList = new ArrayList<List<Integer>>();
        for(int i = 0; i<=rowIndex;i++){
            List<Integer> intList = new ArrayList<Integer>();
            for(int j=0;j<=i;j++){
                if(j == 0|| j==i)
                    intList.add(1);
                else{
                    intList.add(mList.get(i-1).get(j)+mList.get(i-1).get(j-1));
                }
            }
            mList.add(intList);
        }
        return mList.get(rowIndex);
}
```


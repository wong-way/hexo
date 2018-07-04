---
title: leetcode-763
date: 2018-06-09 18:11:59
categories: leetcode
tags:
- leetcode
- oj
- greedy
- two pointer
description: leetcode No763. Partition Labels(Greedy)
---
# leetcode-763 Partition Labels

## 问题描述

>A string S of lowercase letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.
>```text
>Example 1:
>Input: S = "ababcbacadefegdehijhklij"
>Output: [9,7,8]
>Explanation:
>The partition is "ababcbaca", "defegde", "hijhklij".
>This is a partition so that each letter appears in at most one part.
>A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
>```
>Note:
S will have length in range [1, 500].
S will consist of lowercase letters ('a' to 'z') only.

## 解题思路

遍历整个字符串，对每`i`位字符`ch`，查找最后一个与他相同的的字符`String[j]==ch`，则区间`[i,j]`即为字符`ch`所在的区间。这里有两种情况:

* `[i,j]`在之前出现的另一个字符的子区间中，则不另行处理
* `[i,j]`不在之前某个字符的子区间，则新建一个区间
* `[i,j]`与之前某个字符的子区间存在交集，则扩充之间的区间

## 解题代码

```java
public List<Integer> partitionLabels(String S) {
    List<Integer> result = new ArrayList<>();
    int start = 0;
    int end = 0;
    for (int i = 0; i < S.length(); i++) {

        for (int j = S.length() - 1; j > i; j--) {
            if (S.charAt(j) == S.charAt(i)) {
                if (j > end) {
                    end = j;
                }
                break;
            }
        }
        if (i == end) {
            result.add(end - start + 1);
            start = i + 1;
            end = i + 1;
        }
    }
    return result;

}
```

## 评论区优质解答

```java
public List<Integer> partitionLabels(String S) {
    if(S == null || S.length() == 0){
        return null;
    }
    List<Integer> list = new ArrayList<>();
    int[] map = new int[26];  // record the last index of the each char

    for(int i = 0; i < S.length(); i++){
        map[S.charAt(i)-'a'] = i;
    }
    // record the end index of the current sub string
    int last = 0;
    int start = 0;
    for(int i = 0; i < S.length(); i++){
        last = Math.max(last, map[S.charAt(i)-'a']);
        if(last == i){
            list.add(last - start + 1);
            start = last + 1;
        }
    }
    return list;
}
```

这种方法和我的思路是类似的，只是我是通过循环来找出每个节点的起始位置，而他是通过一个字典来获取。第二种方法的12行相当于我代码离得内层循环。
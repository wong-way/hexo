---
title: leetcode-856
date: 2018-07-02 12:08:33
categories: leetcode
tags:
- oj
- leetcode
- String
description: leetcode No856.Score of Parentheses
---
# leetcode No856.Score of Parentheses

## 问题描述

>Given a balanced parentheses string S, compute the score of the string based on the following rule:
>
>`()` has score 1
>`AB` has score `A` + `B`, where `A` and `B` are balanced parentheses strings.
>`(A)` has score 2 * `A`, where `A` is a balanced parentheses string.

## 解题思路

对每一个括号`(ABAB)`,将括号中的取出来进行递归计算，并将结果score*2,如果括号中不存在其他字符串就score+1

## 解题代码

```java
public int scoreOfParentheses(String S) {
    int score = 0;
    int i = 0;
    while(i<S.length()){
        if(S.charAt(i)=='('){
            int end = findMatchedparentheses(S,i);
            String sub = S.substring(i+1,end);
            if(sub.length()>0)
                score=score+2*scoreOfParentheses(sub);
            else
                score=score+1;
            i=end+1;
        }
    }
    return score;
}
private int findMatchedparentheses(String S,int start){
    int index=0 ;
    for(int i = start;i<S.length();i++){
        if(S.charAt(i)=='(')
            index ++;
        else
            index --;
        if(index == 0)
            return i;
    }
    return 0;
}
```

## 优化算法

每一个基本的计数单元是`A`,外部的括号都是跟他增加倍数,通过计算每一个基本单元`A`外部有多少个括号来计算这一个基本单元有多少分数。

```java

public int scoreOfParentheses1(String S) {
    int ans = 0, bal = 0;
    for (int i = 0; i < S.length(); ++i) {
        if (S.charAt(i) == '(') {
            bal++;
        } else {
            bal--;
            if (S.charAt(i-1) == '(')
                ans += 1 << bal;
        }
    }

    return ans;
}
```
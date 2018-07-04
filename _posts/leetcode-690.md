---
title: leetcode-690
date: 2018-07-04 20:44:00
categories: leetcode
tags:
- oj
- leetcode
- depth first search
description: leetcode No690.Employee Importance
---
# leetcode No690.Employee Importance

## 问题描述

>You are given a data structure of employee information, which includes the employee's unique id, his importance value and his direct subordinates' id.
>
>For example, employee 1 is the leader of employee 2, and employee 2 is the leader of employee 3. They have importance value 15, 10 and 5, respectively. Then employee 1 has a data structure like [1, 15, [2]], and employee 2 has [2, 10, [3]], and employee 3 has [3, 5, []]. Note that although employee 3 is also a subordinate of employee 1, the relationship is not direct.
>
>Now given the employee information of a company, and an employee id, you need to return the total importance value of this employee and all his subordinates.
>```text
>Example 1:
>Input: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
>Output: 11
>Explanation:
>Employee 1 has importance value 5, and he has two direct subordinates: employee 2 and employee 3. They both have importance value 3. So the total importance value of employee 1 is 5 + 3 + 3 = 11.
>```
>Note:
>One employee has at most one direct leader and may have several subordinates.
>The maximum number of employees won't exceed 2000.

## 解题思路

这道题本质上是一个n叉树，因此只需要按照前序遍历或者后序遍历的方式，对其进行遍历并将所有的权值相加就行了

## 解题代码

```java
public int getImportance(List<Employee> employees, int id) {
    int importance = 0;
    int i ;
    for (i = 0; i < employees.size(); i++) {
        if (employees.get(i).id == id) break;
    }
    Employee boos = employees.get(i);
    importance += boos.importance;
    for (i = 0; i < boos.subordinates.size(); i++) {
        int subordinate = boos.subordinates.get(i);
        importance += getImportance(employees, subordinate);
    }
    return importance;
}
```
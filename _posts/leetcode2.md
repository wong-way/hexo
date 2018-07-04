---
title: leetcode2
date: 2017-08-22 16:48:13
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题top100 第二题 Add Two Numbers
---

## leetcode\_top100\_2

* 问题描述

>You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
>
>You may assume the two numbers do not contain any leading zero, except the number 0 itself.
>
>**Input:** (2 -> 4 -> 3) + (5 -> 6 -> 4)
>**Output:** 7 -> 0 -> 8

* 问题分析

给定两个链表，将链表相加，其中链表的高位是存到链表的后面的。只需要遍历两个链表，将每一个的节点的值相加就行了。注意考虑进位的情况。

* 解题代码

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
 	ListNode newHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = newHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return newHead.next;
}
```


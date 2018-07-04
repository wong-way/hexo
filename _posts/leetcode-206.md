---
title: leetcode-206
date: 2018-04-16 17:19:28
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode第206题，反转链表
---
# leetcode 206 Reverse Linked List

## 问题描述

>Reverse a singly linked list.

## 问题分析

* 单链表的定义

```java
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

* 这个问题只要改每个节点的next指针就行了，我第一感觉递归程序能够解决这个问题，于是采用递归来解决

## 解题代码

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null)
        return head;
    //反转当前节点之后的链表
    ListNode newHead = reverseList(head.next);
    //将当前节点连接到已反转的链表尾部，并将当前节点的next节点赋值为null
    ListNode cur = newHead;
    while(cur.next!=null){
        cur = cur.next;
    }
    cur.next  =head;
    head.next =null;
    return newHead;
}
```

## 总结反思

这种方法效率不高，涉及递归以及循环

## 最佳方式

直接在原始链表上进行改next节点

```java
public ListNode reverseList1(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;//用于控制循环
        //用于改next指向节点
        curr.next = prev;
        prev = curr;

        curr = nextTemp;
    }
    return prev;
}
```
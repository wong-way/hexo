---
title: leetcode-92
date: 2018-04-16 23:30:44
categories: leetcode
tags:
- leetcode
- oj
description: leetcode第92题，Reverse Linked List II
---
# leetcode 92 Reverse Linked List II

## 问题描述

>Reverse a linked list from position m to n. Do it in-place and in one-pass.
For example:
Given 1->2->3->4->5->NULL, m = 2 and n = 4,
return 1->4->3->2->5->NULL.

## 问题分析

这道题是Reverse Linked List 的加强版，仅对指定下标的子链表进行反转，这道题和上一道相似，可以利用递归解决，只需要对m的值进行判断。

* 如果`m==1`，就将相应区间进行反转，后面的区间保持不变。
* 如果 `m>1`，将下一节点，和`m-1`,`n-1`传入进行递归

## 解题代码

```java
public ListNode reverseBetween(ListNode head, int m, int n) {
    ListNode prev = null;
    ListNode cur = head;
    if (head == null || head.next == null)
        return head;
    if (m == 1) {
        int count = 1;
        ListNode temp = null;
        while (count <= n) {
            temp = cur.next;
            cur.next = prev;
            prev = cur;
            cur = temp;
            count++;
        }
        head.next = temp;
        return prev;
    } else {
        head.next = reverseBetween(head.next, m - 1, n - 1);
        return head;
    }
}
```

## 总结

这道题就适合利用递归来解决，提交之后44个用例只用了4ms就全部处理完成，同时速度比97%的提交记录都快

## 非递归解决方案
```java
public ListNode reverseBetween(ListNode head, int m, int n) {
    if(head == null) return null;
    ListNode dummy = new ListNode(0); // create a dummy node to mark the head of this list
    dummy.next = head;
    ListNode pre = dummy; // make a pointer pre as a marker for the node before reversing
    for(int i = 0; i<m-1; i++) pre = pre.next;
    
    ListNode start = pre.next; // a pointer to the beginning of a sub-list that will be reversed
    ListNode then = start.next; // a pointer to a node that will be reversed
    
    // 1 - 2 -3 - 4 - 5 ; m=2; n =4 ---> pre = 1, start = 2, then = 3
    // dummy-> 1 -> 2 -> 3 -> 4 -> 5
    
    for(int i=0; i<n-m; i++)
    {
        start.next = then.next;
        //下面两行是为了替换新的pre.next节点
        then.next = pre.next;
        pre.next = then;
        then = start.next;
    }
    /** 个人理解的逆转方法
    逆转后start节点一定是end节点
    for(int i =0 ;i<n-m;i++)
    {
        ListNode nextNode = then.next;
        then.next = pre.next;
        pre.next = then;
        start.next = nextNode;
        then = nextNode;
    }
    */
    
    // first reversing : dummy->1 - 3 - 2 - 4 - 5; pre = 1, start = 2, then = 4
    // second reversing: dummy->1 - 4 - 3 - 2 - 5; pre = 1, start = 2, then = 5 (finish)
    
    return dummy.next;
    
}
```

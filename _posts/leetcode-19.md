---
title: leetcode-19
date: 2018-07-02 14:49:40
categories: leetcode
tags:
- oj 
- leetcode
- two pointer
description: leetcode No19.Remove Nth Node From End of List
---
# leetcode No19.Remove Nth Node From End of List

## 问题描述

>Given a linked list, remove the n-th node from the end of list and return its head.
>
>Example:
>
>Given linked list: 1->2->3->4->5, and n = 2.
>
>After removing the second node from the end, the linked list becomes 1->2->3->5.
>Note:
>
>Given n will always be valid.
>
>Follow up:
>
>Could you do this in one pass?

## 解题思路

由于链表是单向的，同时要求只在一次遍历中完成，那么怎么确定是倒数第几个十分关键。这里我采用两个指针，一个正常遍历，一个延迟遍历。类似于龟兔赛跑，当兔跑到一定距离之后，乌龟在开始跑

## 解题代码

```java
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode fakerHead =new ListNode(0);
        fakerHead.next =head;
        ListNode faster = head;
        ListNode slower = fakerHead;
        int delay =0;
        while(faster!=null){

            faster= faster.next;
            delay++;
            if(delay >n)
                slower =slower.next;
            if(faster ==null)
                slower.next = slower.next.next;

        }
        return fakerHead.next;
    }
```

## 评论区代码

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    
    ListNode start = new ListNode(0);
    ListNode slow = start, fast = start;
    slow.next = head;
    
    //Move fast in front so that the gap between slow and fast becomes n
    for(int i=1; i<=n+1; i++)   {
        fast = fast.next;
    }
    //Move fast to the end, maintaining the gap
    while(fast != null) {
        slow = slow.next;
        fast = fast.next;
    }
    //Skip the desired node
    slow.next = slow.next.next;
    return start.next;
}
```
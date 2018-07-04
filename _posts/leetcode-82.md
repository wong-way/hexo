---
title: leetcode-82
date: 2018-07-02 18:59:07
categories: leetcode
tags:
- oj 
- leetcode
- two pointer
description: leetcode No82. Remove Duplicates from Sorted List II
---
# leetcode No82. Remove Duplicates from Sorted List II

## 问题描述

>Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.
>
>Example 1:
>
>Input: 1->2->3->3->4->4->5
>Output: 1->2->5
>Example 2:
>
>Input: 1->1->1->2->3
>Output: 2->3

## 解题思路

整体思路是通过计数法，对每一个不同的值进行计数，如果数量为1，则加入到新链表中；如果数量大于1则抛弃

## 解题代码

```java
 public ListNode deleteDuplicates(ListNode head) {
    if(head ==null) return head;
    
    ListNode prev = head;
    ListNode steper = head;
    ListNode newHead = new ListNode(-1);
    ListNode counter = newHead;
    int count =0;
    while(steper!=null){
        if(steper.val == prev.val) {
            count++;
        }else{
            if(count>1){
                prev = steper;
                count = 1;
            }else{
                counter.next = new ListNode(prev.val);
                counter = counter.next;
                prev = steper;
                count = 1;
            }
        }
        steper = steper.next;
    }
    if(prev.val!=counter.val&&count<=1)
        counter.next = new ListNode(prev.val);
    return newHead.next;
}
```

## 评论区优质代码

与我解答方式相似
```java
public ListNode deleteDuplicates(ListNode head) {
        if(head==null) return null;
        ListNode FakeHead=new ListNode(0);
        FakeHead.next=head;
        ListNode pre=FakeHead;
        ListNode cur=head;
        while(cur!=null){
            while(cur.next!=null&&cur.val==cur.next.val){
                cur=cur.next;
            }
            if(pre.next==cur){
                pre=pre.next;
            }
            else{
                pre.next=cur.next;
            }
            cur=cur.next;
        }
        return FakeHead.next;
    }
```

递归解决方式

```java
public ListNode deleteDuplicates(ListNode head) {
    if (head == null) return null;
    
    if (head.next != null && head.val == head.next.val) {
        while (head.next != null && head.val == head.next.val) {
            head = head.next;
        }
        return deleteDuplicates(head.next);
    } else {
        head.next = deleteDuplicates(head.next);
    }
    return head;
}
```
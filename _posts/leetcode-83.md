---
title: leetcode_83
date: 2017-07-12 15:37:27
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第83道 Remove Duplicates from Sorted List
---

### leetcode\_83_Remove Duplicates from Sorted List

> Given a sorted linked list, delete all duplicates such that each element appear only *once*.

> For example,
>
> Given `1->1->2`, return `1->2`.
>
> Given `1->1->2->3->3`, return `1->2->3`.

**问题分析**

这个题目和之前那个在数组中，去除重复的元素相似，只要每次找到下一个不同元素，然后指到下一个就行了

**解题思路**

* 循环，遍历整个链表。
* 找到下一个`value`不同的值。
* 连接到`newHead`后面

**解题代码**

```java
public static ListNode deleteDuplicates(ListNode head) {
        if(head == null)
            return null;
        ListNode newHead =new ListNode(head.val);
        ListNode temp=newHead;
        while(head!=null){
            if(head.val != temp.val){
                temp.next = new ListNode(head.val);
                temp=temp.next;
            }
            head=head.next;
        }
        return newHead;
}
```

**示例代码**

```java
public static ListNode deleteDuplicates1(ListNode head){
        if(head == null || head.next == null)
            return head;
        head.next = deleteDuplicates(head.next);
        return head.val == head.next.val ? head.next : head;
    }
```


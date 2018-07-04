---
title: leetcode_160
date: 2017-07-25 16:18:44
categories: leetcode
tags:
- oj
- leetcode
description: leetcode第160题，Intersection of Two Linked Lists
---

## leetcode\_160\_Intersection of Two Linked Lists

>Write a program to find the node at which the intersection of two singly linked lists begins.
>
>For example, the following two linked lists:
>
>```
>A:          a1 → a2
>                   ↘
>                     c1 → c2 → c3
>                   ↗            
>B:     b1 → b2 → b3
>
>```
>
>begin to intersect at node c1.
>
>**Notes:**
>
>- If the two linked lists have no intersection at all, return `null`.
>- The linked lists must retain their original structure after the function returns.
>- You may assume there are no cycles anywhere in the entire linked structure.
>- Your code should preferably run in O(n) time and use only O(1) memory.

1. ## **问题分析**

这道题我觉得是我做了几十道题之后我觉得最巧妙的一道题，算法贼恐怖。

​	一开始，很自然想到遍历两个链表，但是这种方式的时间复杂的事O(mn)，看了评论区的答案之后，恍然大悟。太厉害了吧。这个方法将两个链表分别遍历两次，先将`a =headA`,`b= headB`，遍历完一遍之后令`a = headB`，`b = headA`。因为，两个链表的交集部分长度是一样的，这样可以保证我们在比对时，交集部分是同步的，即是对齐的。

​	在上面的例子中，遍历一遍之后` a = b1` , `b= c3`，再经过一次循环，`a =b2` ,` b=a1`。这样到后面就可以找到交集的部分。

2. ## **解题代码**

```java
 public static ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        //boundary check
        if(headA == null || headB == null) return null;
        ListNode a = headA;
        ListNode b = headB;
        //if a & b have different len, then we will stop the loop after second iteration
        while( a != b){
            //for the end of first iteration, we just reset the pointer to the head of another linkedlist
            a = a == null? headB : a.next;
            b = b == null? headA : b.next;
        }
        return a;
 }
```


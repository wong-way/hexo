---
title: leetcode-203
date: 2018-04-11 14:47:36
tags: leetcode
---
# Leetcode 203

## 问题

>Remove all elements from a linked list of integers that have value val.
Example
Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6
Return: 1 --> 2 --> 3 --> 4 --> 5

## 分析

去除链表中值为val的节点，同时保证其他节点的顺序保持原样。

## 解决方式

采用两个指针，一个为pre一个为next,每次判断next指向的节点的值是否与val相等，如果相等next指针后移一位，同时改变pre的下一个节点为next指向的当前节点。

## 解题代码

```java
public ListNode removeElements(ListNode head, int val) {
    ListNode newHead =new ListNode(0);
    ListNode temp = new ListNode(0) ;
    newHead.next = temp;
    ListNode next = head;
    while(next!=null){
        if(next.val!=val){
            temp.next =new ListNode(next.val);
            temp = temp.next;
        }
        next=next.next;
    }
    return newHead.next.next;
}

```

## 总结

* 一开始我是直接想用两个指针来进行操作，但是发现最后游标会跟着移动，无法直接返回头部，于是在前面加了两个没用的节点，这样有额外的空间开销，但是可以记录头部指针的位置

* 由于在解答过程中，采用了一个额外的链表，而不是在原来的链表上进行操作，空间开销大

* 没有考虑到使用递归来处理，思维方式有问题

## 优质解答
 
### 方法一

* 这个方式相当于我的改进版，在原链表的基础之上改next指针，节约空间开销。

* 也像我一样采用一个`fakerHead`来记录头部位置，这也是我考虑比较好的地方。

```java
public ListNode removeElements(ListNode head, int val) {
    ListNode fakeHead = new ListNode(-1);
    fakeHead.next = head;
    ListNode cur = fakeHead;

    while(cur.next!=null){
        if(cur.next.val==val){
            cur.next = cur.next.next;
        }else{
            cur = cur.next;
        }
    }
    return fakeHead.next;
}
```

## 方法二

递归程序，简洁明了

```java
public ListNode removeElements(ListNode head, int val) {
    if (head == null) return null;
    head.next = removeElements(head.next, val);
    return head.val == val ? head.next : head;
}
```
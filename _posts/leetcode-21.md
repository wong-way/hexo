---
title: leetcode_21
date: 2017-07-06 15:00:05
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第21道 Merge Two Sorted Lists    
---

> Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

将两个已经排好序的列表合并成为一个有序的列表，我想到有两种方法：

* 第一种是通过循环比较两个列表队首的元素，然后添加到新的队列里面
* 第二种是采用递归的方法是，先把两个列表种最小的元素找出来，然后再用剩下来的队列进行递归调用

下面展示第二种方法的代码：

辅助用的节点类：

```java
public class ListNode{
    public int val;
    public ListNode next;
    public ListNode(int x){val = x;}
}
```

合并列表

```java
public class Solution21 {
    public static ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode newHead;
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }
        if (l1.val > l2.val) {
            newHead = l2;
            newHead.next = (mergeTwoLists(l1, l2.next));
        } else {
            newHead = l1;
            newHead.next = (mergeTwoLists(l1.next, l2));
        }
        return newHead;
    }
}
```

测试代码

```java
public static void main(String[] args) {
  ListNode l1 = new ListNode(1);
  ListNode l2 = new ListNode(4);
  ListNode l3 = new ListNode(6);

  ListNode n1 = new ListNode(2);
  ListNode n2 = new ListNode(3);
  ListNode n3 = new ListNode(4);
  ListNode n4 = new ListNode(7);
  l1.next = l2;
  l2.next = l3;
  n1.next = n2;
  n2.next = n3;
  n3.next = n4;
  ListNode result = mergeTwoLists(l1,n1);
  while(result!=null){
    System.out.println(result.val);
    result = result.next;
  }
}
```


---
title: leetcode141
date: 2017-08-22 16:59:00
categories: leetcode
tags: 
- leetcode
- oj
description: leetcode算法题第141道 Linked List Cycle 
---

## leetcode\_top100\_141

* 问题描述

>Given a linked list, determine if it has a cycle in it.
>
>Follow up:
>Can you solve it without using extra space?

* 问题分析

  之前遇到过一道类似的题目，是让我们判断两个链表是不是有相同的部分。当时的做法是对两个链表进行两次遍历。在第一次遍历结束之后，让`a.next =b ,b.next= a`。这道题我们可以采取相同思路来解决这个问题，利用两个节点`walker`和`runner`，我们让`walker`每次走一步然后让`runner`每次走两步，如果存在环，那么就一定会存在`walker=runner`的情况

* 解题代码

  ```java
  /**
   * Definition for singly-linked list.
   * class ListNode {
   *     int val;
   *     ListNode next;
   *     ListNode(int x) {
   *         val = x;
   *         next = null;
   *     }
   * }
   */
  public class Solution {
      public boolean hasCycle(ListNode head) {
          if(head==null) return false;
          ListNode walker = head;
          ListNode runner = head;
          while(runner.next!=null && runner.next.next!=null) {
              walker = walker.next;
              runner = runner.next.next;
              if(walker==runner) return true;
          }
          return false;
      }
  }
  ```

  ​
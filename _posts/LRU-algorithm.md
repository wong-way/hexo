---
title: LRU algorithm
date: 2018-08-23 22:37:50
categories: java
tags: 
- leetcode
- oj
- algorithm
description: 最近最少使用算法（Least Recently Used）的设计和实现 
---
# Least Recently Used

## 0.问题引入
在操作系统中关于缓存替换内容部分，学习了包括LRU、FIFO、RR等缓存替换算法，当时都是手写计算过程没有使用代码实现，因为一次课程设计中，简单代理服务器的设计与实现，我使用了LRU的思想进行缓存的管理。当时是如果缓存空间满，则清楚最近最少使用的50%的缓存。最近再次了解到Google的Guava中的RateLimiter以及缓存管理等，于是在这里做一个LRU算法的简单实现

## 1.LRU算法介绍
[LRU算法思想](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)
>Discards the least recently used items first. This algorithm requires keeping track of what was used when, which is expensive if one wants to make sure the algorithm always discards the least recently used item. General implementations of this technique require keeping "age bits" for cache-lines and track the "Least Recently Used" cache-line based on age-bits. In such an implementation, every time a cache-line is used, the age of all other cache-lines changes. LRU is actually a family of caching algorithms with members including 2Q by Theodore Johnson and Dennis Shasha,[3] and LRU/K by Pat O'Neil, Betty O'Neil and Gerhard Weikum.[4]
>
>The access sequence for the below example is A B C D E F.
[image](https://en.wikipedia.org/wiki/Cache_replacement_policies#/media/File:Lruexample.png)

## 2.实现思路
在缓存管理中，缓存大小是固定的块，但是窗口大小是固定的。因此我利用链表实现一个窗口，维护链表的长度。
* 如果使用到链表中已有的块，则调整链表结构，将该块移动至链表的尾部
* 如果有新增的块，如果链表长度还没有达到预设长度则添加到链表的尾部，如果链表已经达到预设长度，则将替换最近最少使用的块**在此处即为头指针**

## 3.实现代码

```java
public class LRUMap<K, V> {
    //当前缓存的节点数
    private int currentSize;
    //最多缓存的节点数
    private int maxSize;
    //头节点
    private Node<K, V> header;
    //尾节点
    private Node<K, V> tailer;


    public LRUMap(int size) {
        header = new Node<>();
        tailer = header;
        this.maxSize = size;
        this.currentSize = 0;

    }

    /**
     * 将节点放进缓存里
     *
     * @param key   节点的键值
     * @param value 节点的值
     */
    public void put(K key, V value) {
        if (currentSize < maxSize) {
            currentSize++;
            Node<K, V> newNode = new Node<>(key, value);
            tailer.next = newNode;
            tailer = newNode;
        } else {
            //替换节点
            System.out.println("替换:" + header.next.getValue());
            header = header.next;
            tailer.next = new Node<>(key, value);
            tailer = tailer.next;
        }
    }

    /**
     * 得到键值为key的节点，并更新该节点的位置，将该节点调整到末端
     *
     * @param key 键值
     * @return 键值为key的节点
     * @throws Exception 没有找到节点异常
     */

    public Node<K, V> get(K key) throws Exception {
        Node<K, V> node = getNode(key);
        if (node != null) {
            moveToTail(node);
        } else {
            throw new Exception("没有键值为：" + key + "的缓存节点");
        }
        return node;
    }

    /**
     * 将节点调整到窗口的末端
     *
     * @param node 待调整的窗口
     */

    private void moveToTail(Node<K, V> node) {

        Node<K, V> steper = header;
        Node<K, V> prev = header;
        while (steper.next != null) {
            if (steper.next.getKey() == node.getKey()) {
                prev.next = steper.next.next;
            } else {
                prev = prev.next;
            }
            steper = steper.next;
        }
        steper.next = node;
        tailer = node;
        tailer.next = null;

    }

    /**
     * 得到键为key的节点
     *
     * @param key 键值
     * @return 如果存在则返回节点，如果不存在则返回null
     */
    private Node<K, V> getNode(K key) {

        Node<K, V> node = header.next;
        while (node != tailer) {
            if (node.getKey() == key) break;
            node = node.next;
        }
        if (node == tailer) {
            return null;
        }
        return node;
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        Node<K, V> node = header.next;
        while (node != null) {
            sb.append(node.getKey()).append(":")
                    .append(node.getValue())
                    .append("-->");

            node = node.next;
        }

        return sb.toString();

    }

    public void useChache(K k, V v) {
        try {
            //缓存命中
            get(k);
            System.out.println("缓存命中");
        } catch (Exception e) {
            //缓存未命中
            System.out.println("缓存未命中");

            put(k, v);
        }
    }

    public static void main(String[] args) {
        LRUMap<Integer, String> map = new LRUMap<>(4);
        map.useChache(4, "cache4");
        map.useChache(3, "cache3");
        map.useChache(4, "cache4");
        map.useChache(1, "cache1");
        map.useChache(5, "cache5");
        map.useChache(2, "cache2");
        map.useChache(4, "cache4");
        map.useChache(1, "cache1");
        map.useChache(2, "cache2");
        map.useChache(6, "cache6");
        System.out.println(map.toString());

    }
}

```

## leetcode No146.LRU Cache
### 问题描述

>Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.
>
>get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.
### 解题思路
这道题的要求和我之前实现不大一样，之前按照我的设计是直接请求某一个key值，如果存在就调整位置，并返回，如果不存在则把他加入到应该放的位置。而这里分为两种操作，`get`和`put`两种操作都算作是访问了cache，并且在put时，如果`key`已存在则需要更新他的值。整体思路还是按照之前的。
### 解题代码

```java
public class LRUCache {
    private int capacity;
    private int currentSize;
    private Node head;
    private Node tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        head = new Node();
        tail = head;
        currentSize = 0;
    }

    public int get(int key) {
        Node stepper = head;
        Node curr = null;
        while (stepper.next != null) {
            if (stepper.next.key == key) {
                curr = stepper.next;
                stepper.next = stepper.next.next;

            }else{
                stepper = stepper.next;
            }

        }
        if (curr == null) {
            return -1;
        } else {
            stepper.next = curr;
            tail = curr;
            tail.next = null;
            return curr.value;
        }


    }

    public void put(int key, int value) {
        if(get(key)!=-1){
            Node newNode = new Node(key, value);
            currentSize++;
            tail.next = newNode;
            tail = tail.next;
            if (currentSize > capacity) {
                //将头指针后移
                head = head.next;
            }
        }else{
            Node temp = head;
            while (temp != null) {
                if (temp.key == key) {
                    temp.value=value;
                }
                temp=temp.next;
            }
        }

    }

    private class Node {
        public int key;
        public int value;
        public Node next;

        public Node() {
        }

        public Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    public static void main(String[] args) {
        LRUCache cache = new LRUCache(1);
        cache.put(2, 1);

        cache.get(2);
        cache.put(3, 2);
        cache.get(2);
        cache.get(3);

    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```
---
title: leetcode-841
date: 2018-06-11 11:18:04
categories: leetcode
tags:
- oj
- leetcode
- graph
description: leetcode No.841 Keys and Rooms
---
# leetcode No.841 Keys and Rooms

## 问题描述

>There are N rooms and you start in room 0.  Each room has a distinct number in 0, 1, 2, ..., N-1, and each room may have some keys to access the next room. 
>
>Formally, each room i has a list of keys rooms[i], and each key rooms[i][j] is an integer in [0, 1, ..., N-1] where N = rooms.length.  A key rooms[i][j] = v opens the room with number v.
>
>Initially, all the rooms start locked (except for room 0). 
>
>You can walk back and forth between rooms freely.
>
>Return true if and only if you can enter every room.
>```text
>Example 1:
>
>Input: [[1],[2],[3],[]]
>Output: true
>Explanation:  
>We start in room 0, and pick up key 1.
>We then go to room 1, and pick up key 2.
>We then go to room 2, and pick up key 3.
>We then go to room 3.  Since we were able to go to every room, we return true.
>Example 2:
>
>Input: [[1,3],[3,0,1],[2],[0]]
>Output: false
>Explanation: We can't enter the room with number 2.
>```

## 解题思路

这道题是一个图的遍历问题，采用深度优先或者广度优先都可以解决，我这里采用深度优先，使用递归来解决。
通过一个辅助函数，判断从某个房间开始，能否走通所有的房间，函数ADT为`visitRooms(List<List<Integer>> rooms, int start, List<Integer> visitedRooms)`

## 解题代码

```java
public boolean canVisitAllRooms(List<List<Integer>> rooms) {
    List<Integer> visited = new ArrayList<>();
    visited.add(0);
    return visitRooms(rooms, 0, visited);
}

public boolean visitRooms(List<List<Integer>> rooms, int start, List<Integer> visitedRooms) {
    boolean result = false;
    if (visitedRooms.size() == rooms.size())
        return true;

    for (int room :
            rooms.get(start)) {
        if (!visitedRooms.contains(room)) {
            visitedRooms.add(room);
            result = result || visitRooms(rooms, room, visitedRooms);
        }
    }

    return result;
}
```

## 评论区优质解答与分析

```java
HashSet<Integer> enteredRooms = new HashSet<>();

public boolean canVisitAllRooms(List<List<Integer>> rooms) {
    enterRoom(0, rooms);
    return enteredRooms.size() == rooms.size();
}

private void enterRoom(int roomId, List<List<Integer>> rooms) {
    enteredRooms.add(roomId);
    List<Integer> keysInRoom = rooms.get(roomId);
    for (int key: keysInRoom)
        if (!enteredRooms.contains(key))
            enterRoom(key, rooms);
}
```

```java
public boolean canVisitAllRooms(List<List<Integer>> rooms) {
    Set<Integer> visited=new HashSet<>();
    addKey(0, rooms,visited);
    return visited.size() == rooms.size();
}

private void addKey(int room, List<List<Integer>> rooms,Set<Integer> visited) {
    visited.add(room);
    for (int key: rooms.get(room))
        if (!visited.contains(key)) addKey(key, rooms,visited);
    return;
}
```
上述两种方式跟我思路一致，只是实现细节有所差别

C++解法：BFS

```C++
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        unordered_set<int> visited;
        queue<int> to_visit;
        to_visit.push(0);
        while(!to_visit.empty()) {
            int curr = to_visit.front();
            to_visit.pop();
            visited.insert(curr);
            for (int k : rooms[curr]) if (visited.find(k) == visited.end()) to_visit.push(k);
        }
        return visited.size() == rooms.size();
    }
};
```

C++解法：DFS

```C++
class Solution {
    void dfs(vector<vector<int>>& rooms, unordered_set<int> & keys, unordered_set<int> & visited, int curr) {
        visited.insert(curr);
        for (int k : rooms[curr]) keys.insert(k);
        for (int k : keys) if (visited.find(k) == visited.end()) dfs(rooms, keys, visited, k);
    }
    
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        unordered_set<int> keys;
        unordered_set<int> visited;
        dfs(rooms, keys, visited, 0);
        return visited.size() == rooms.size();
    }
};
```
---
title: leetcode-797
date: 2018-06-08 17:09:54
categories: leetcode
tags:
- leetcode
- oj
- dynamic programming
- graph
description: leetcode第797题All Paths From Source to Target（动态规划，图）
---
# leecode-797 All Paths From Source to Target

## 问题描述

>Given a directed, acyclic graph of N nodes.  Find all possible paths from node 0 to node N-1, and return them in any order.
>The graph is given as follows:  the nodes are 0, 1, ..., graph.length - 1.  graph[i] is a list of all nodes j for which the edge (i, j) exists.
>```text
>Example:
>Input: [[1,2], [3], [3], []] 
>Output: [[0,1,3],[0,2,3]] 
>Explanation: The graph looks like this:
>0--->1
>|    |
>v    v
>2--->3
>There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.
>```

## 问题分析

* 第一时间，我想到是循环。遍历每个节点`i`，如果`i`到`j`有边存在，则在`target[j]`中添加`target[i]+[i,j]`,注意`target[i]`可能有多条路径，需要进行循环添加

* 在写代码中，我发现上面的方法太繁琐，于是想到这道题可以很好地的用递归解决。遍历每个节点，对任意`[i,j]`均进行`findPathFrom(j)`,然后将返回的结果添加路径`[i,j]`

## 解题代码（递归方式）

```java
 public List<List<Integer>> allPathsSourceTarget(int[][] graph){
    return allPathsFromSource(graph, 0);
}
public List<List<Integer>> allPathsFromSource(int[][] graph,int source){
    List<List<Integer>> result = new ArrayList<>();
    if (source == graph.length - 1) {
        List temp = new ArrayList();
        temp.add(source);
        result.add(temp);

        return result;
    }
    for (int i = 0; i < graph[source].length; i++) {
        List<List<Integer>> part = allPathsFromSource(graph, graph[source][i]);
        for (int j = 0; j < part.size(); j++) {
            List temp = new ArrayList();
            temp.add(source);
            temp.addAll(part.get(j));
            result.add(temp);
        }

    }
    return result;
}
```

## 解题代码（循环方式）

```java
 public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
    List<List<List<Integer>>> result = new ArrayList<>();
    for (int i = 0; i < graph.length; i++) {
        List<List<Integer>> list = new ArrayList<>();
        result.add(list);
    }
    for (int i = 0; i < graph.length; i++) {
        int source = i;
        for (int j = 0; j < graph[i].length; j++) {
            int target = graph[i][j];
            if (source == 0) {
                List<Integer> temp = new ArrayList<Integer>();
                temp.add(source);
                temp.add(target);
                result.get(target).add(temp);
            } else {
                for (int k = 0; k < result.get(source).size(); k++) {
                    List<Integer> temp = new ArrayList<Integer>();
                    temp.addAll(result.get(source).get(k));
                    temp.add(target);
                    result.get(target).add(temp);
                }
            }

        }
    }
    return result.get(graph.length - 1);
}
```

## 讨论区优质解答

```java
public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new ArrayList<>();

    path.add(0);
    dfsSearch(graph, 0, res, path);

    return res;
}

private void dfsSearch(int[][] graph, int node, List<List<Integer>> res, List<Integer> path) {
    if (node == graph.length - 1) {
        res.add(new ArrayList<Integer>(path));
        return;
    }

    for (int nextNode : graph[node]) {
        path.add(nextNode);
        dfsSearch(graph, nextNode, res, path);
        path.remove(path.size() - 1);
    }
}
```
分析来看，我的解决方式是一种自底而上的方式，上一层的递归都依赖于下一层计算的结果。
而评论的结果则是自顶向上的方式，下一层都依赖于已有的上一层的path，这种方式和我想的循环解决就很相似

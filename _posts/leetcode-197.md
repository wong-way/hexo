---
title: leetcode-197
date: 2018-04-11 19:13:53
tags: 
  - leetcode
  - sql
---
# leetcode-196

## 问题

>Write a SQL query to delete all duplicate email entries in a table named Person, keeping only unique emails based on its smallest Id.
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Id is the primary key column for this table.
For example, after running your query, the above Person table should have the following rows:
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+

## 分析

这道题，让我们删除重复的邮箱，我们只需要将`person`表进行自连接，然后设置筛选条件就可以了

## 解题代码

```sql
DELETE p1
FROM Person p1, Person p2
WHERE p1.Email = p2.Email AND p1.Id > p2.Id
```

# leetcode-197

## 问题
>Given a Weather table, write a SQL query to find all dates' Ids with higher temperature compared to its previous (yesterday's) dates.
+---------+------------+------------------+
| Id(INT) | Date(DATE) | Temperature(INT) |
+---------+------------+------------------+
|       1 | 2015-01-01 |               10 |
|       2 | 2015-01-02 |               25 |
|       3 | 2015-01-03 |               20 |
|       4 | 2015-01-04 |               30 |
+---------+------------+------------------+
For example, return the following Ids for the above Weather table:
+----+
| Id |
+----+
|  2 |
|  4 |
+----+

## 分析

这道题让我们选出比前一天温度高的id，先做一个表的自连接，然后在where子句中增减限制条件，w1.Temperature > w2.Temperature 以及 DATEDIFF(w1.date, w2.date) = 1

## 解题代码

```sql
SELECT w1.Id
FROM Weather w1, Weather w2
WHERE w1.Temperature > w2.Temperature AND DATEDIFF(w1.date, w2.date) = 1
```
---
title: SQL-1
date: 2018-04-10 21:42:03
tags:
---
# 获取所有员工当前的manager
### 问题
>获取所有员工当前的manager，如果当前的manager是自己的话结果不显示，当前表示to_date='9999-01-01'。
结果第一列给出当前员工的emp_no,第二列给出其manager对应的manager_no。
```sql
CREATE TABLE `dept_emp` (
`emp_no` int(11) NOT NULL,
`dept_no` char(4) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));
CREATE TABLE `dept_manager` (
`dept_no` char(4) NOT NULL,
`emp_no` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));
```
### 分析


1. 再用WHERE限制当前员工与当前经理的条件，即 m.to_date 等于 '9999-01-01' 、e.to_date 等于 '9999-01-01' 、 e.emp_no 不等于m.emp_no、e.dept_no = m.dept_no
2. 为了增强代码可读性，将dept_emp用别名e代替，dept_manager用m代替，最后根据题意将e.emp_no用别名manager_no代替后输出

### 代码
```sql
SELECT e.emp_no, m.emp_no as manager_no
FROM dept_emp e, dept_manager m
WHERE e.dept_no = m.dept_no
    AND e.emp_no!=m.emp_no
    AND e.to_date ='9999-01-01'
    AND m.to_date='9999-01-01';
```
# 获取所有非manager的员工emp_no
### 问题
>获取所有非manager的员工emp_no
```sql
CREATE TABLE `dept_manager` (
`dept_no` char(4) NOT NULL,
`emp_no` int(11) NOT NULL,
`from_date` date NOT NULL,
`to_date` date NOT NULL,
PRIMARY KEY (`emp_no`,`dept_no`));
CREATE TABLE `employees` (
`emp_no` int(11) NOT NULL,
`birth_date` date NOT NULL,
`first_name` varchar(14) NOT NULL,
`last_name` varchar(16) NOT NULL,
`gender` char(1) NOT NULL,
`hire_date` date NOT NULL,
PRIMARY KEY (`emp_no`));
```
### 分析
* 使用NOT IN选出在employees但不在dept_manager中的emp_no记录
* 先使用LEFT JOIN连接两张表，再从此表中选出dept_no值为NULL对应的emp_no记录
### 代码
```sql
SELECT emp_no
FROM employees as e
WHERE e.emp_no NOT IN (SELECT emp_no
FROM dept_manager)

SELECT employees.emp_no
FROM employees LEFT JOIN dept_manager
    ON employees.emp_no = dept_manager.emp_no
WHERE dept_no IS NULL
```
---
layout: "post"
title: "Leetcode 177: Nth Highest Salary"
date: "2018-07-28 15:42"
tags:
  - Leetcode
  - SQL
---

# Question
Write a SQL query to get the $$n^{th}$$ highest salary from the Employee table.

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

For example, given the above Employee table, the nth highest salary where n = 2 is 200. If there is no nth highest salary, then the query should return null.

```
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+

```

# Solution
`DISTINCT` clause considers `null` as a unique value.

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
SET N = N - 1;
  RETURN (
      # Write your MySQL query statement below.
      select distinct Salary from Employee order by Salary desc limit 1 offset N
  );
END
```

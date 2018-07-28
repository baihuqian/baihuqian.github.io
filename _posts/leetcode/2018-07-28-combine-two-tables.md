---
layout: post
title: 'Leetcode 175: Combine Two Tables'
date: '2018-07-28 15:31'
tags:
  - Leetcode
  - SQL
---

# Question
Table: `Person`
```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId is the primary key column for this table.
```

Table: `Address`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId is the primary key column for this table.
```

Write a SQL query for a report that provides the following information for each person in the `Person` table, regardless if there is an address for each of those people:

```
FirstName, LastName, City, State
```

# Solution
Use outer join.

```sql
# Write your MySQL query statement below
SELECT Person.FirstName as FirstName, Person.LastName as LastName, Address.City as City, Address.State as State
from Person left outer join Address on Person.PersonId = Address.PersonId;
```

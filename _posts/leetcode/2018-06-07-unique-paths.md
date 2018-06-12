---
layout: post
title: 'Leetcode 62: Unique Paths'
date: '2018-06-07 22:14'
tags:
  - Leetcode
---

# Question
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

Note: m and n will be at most 100.

Example 1:
```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```

Example 2:
```
Input: m = 7, n = 3
Output: 28
```

# Solution
It is $${m+n-2\choose m-1}$$, which can be reduced to $$\frac{n\times \dots \times (m+n-2)}{1\times \dots \times (m-1)}$$.

```python
class Solution:
    def uniquePaths(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        if m < n:
            m, n = n, m

        numerator = 1
        denominator = 1
        for i in range(n, m + n - 1):
            numerator *= i
        for i in range(1, m):
            denominator *= i

        return numerator // denominator

```

---
layout: post
title: 'Leetcode 77: Combinations'
date: '2018-06-13 22:53'
tags:
  - Leetcode
  - Backtracking
---

# Question
Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

Example:
```
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

# Solution
```python
class Solution:
    def combine(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: List[List[int]]
        """
        if k > n:
            return []
        if k == n:
            return [[i for i in range(1, n + 1)]]

        res = list()
        self.backtracking(res, list(), 0, k, n)
        return res

    def backtracking(self, res, partial, last_i, remaining, n):
        if remaining == 0:
            res.append(partial)
        else:
            for i in range(last_i + 1, n + 1):
                new_partial = partial + [i]
                self.backtracking(res, new_partial, i, remaining - 1, n)
```

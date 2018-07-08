---
layout: "post"
title: "Leetcode 216: Combination Sum III"
date: "2018-07-06 21:43"
tags:
  - Leetcode
  - Backtracking
---

# Question
Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

Note:

* All numbers will be positive integers.
* The solution set must not contain duplicate combinations.

Example 1:

```
Input: k = 3, n = 7
Output: [[1,2,4]]
```

Example 2:

```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

# Solution
```python
class Solution(object):
    def combinationSum3(self, k, n):
        """
        :type k: int
        :type n: int
        :rtype: List[List[int]]
        """
        if n < sum(range(k + 1)):
            return []
        res = []
        self.backtracking(k, n, [], res, 0)
        return res

    def backtracking(self, k, n, partial, res, last):
        if k == 0:
            if n == 0:
                res.append(partial)
        elif k == 1:
            if last + 1 <= n <= 9:
                res.append(partial + [n])
        else:
            for i in range(last + 1, min(9, n)):
                self.backtracking(k - 1, n - i, partial + [i], res, i)
```

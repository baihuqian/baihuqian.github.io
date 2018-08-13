---
layout: "post"
title: "Leetcode 357: Count Numbers with Unique Digits"
date: "2018-08-12 19:57"
tags:
  - Leetcode
---

# Question
Given a non-negative integer n, count all numbers with unique digits, x, where 0 ≤ x < 10n.

Example:
```
Input: 2
Output: 91
Explanation: The answer should be the total numbers in the range of 0 ≤ x < 100,
             excluding 11,22,33,44,55,66,77,88,99
```


# Solution
```python
class Solution:
    def countNumbersWithUniqueDigits(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n == 0:
            return 1
        if n == 1:
            return 10
        res = 10

        for i in range(2, min(n + 1, 10)):
            count = 9
            for j in range(i - 1):
                count *= (9 - j)
            res += count

        return res
```

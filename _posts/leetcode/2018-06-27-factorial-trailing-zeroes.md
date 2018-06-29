---
layout: "post"
title: "Leetcode 172: Factorial Trailing Zeroes"
date: "2018-06-27 22:11"
tags:
  - Leetcode
---

# Question
Given an integer n, return the number of trailing zeroes in n!.

Example 1:
```
Input: 3
Output: 0
Explanation: 3! = 6, no trailing zero.
```

Example 2:
```
Input: 5
Output: 1
Explanation: 5! = 120, one trailing zero.
```

Note: Your solution should be in logarithmic time complexity.

# Solution
Zeros are the product of 5 and 2. Because there are always more 2s than 5s in factorial, we just need to count number of 5s in the factorial. Remember 25 is two 5s, and 125 is three 5s, etc.

```python
class Solution(object):
    def trailingZeroes(self, n):
        """
        :type n: int
        :rtype: int
        """
        fives = 0
        five = 5
        while five <= n:
            fives += n // five
            five *= 5

        return fives

```

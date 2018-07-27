---
layout: "post"
title: "Leetcode 258: Add Digits"
date: "2017-02-05 19:03"
tags: Leetcode
---

# Question
[Question Link](https://leetcode.com/problems/add-digits/description/)

Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

For example:

Given `num = 38`, the process is like: `3 + 8 = 11`, `1 + 1 = 2`. Since 2 has only one digit, return it.

Follow up:
Could you do it without any loop/recursion in O(1) runtime?


# Solution
12,345 = 1 × (9,999 + 1) + 2 × (999 + 1) + 3 × (99 + 1) + 4 × (9 + 1) + 5.

12,345 = (1 × 9,999 + 2 × 999 + 3 × 99 + 4 × 9) + (1 + 2 + 3 + 4 + 5).

Thus, add digits is the same as modulo 9 except when the modulo is 0, the sum is 9.

```python
class Solution(object):
    def addDigits(self, num):
        """
        :type num: int
        :rtype: int
        """
        if num == 0:
            return 0
        mod = num % 9
        return 9 if mod == 0 else mod

```

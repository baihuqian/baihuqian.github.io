---
layout: "post"
title: "Leetcode 202: Happy Number"
date: "2018-07-04 20:13"
tags:
  - Leetcode
---

# Question
Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

# Set Solution
Use a set to store intermediate results to capture loops.

```python
class Solution(object):
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        intermediate = set()
        while True:
            nextN = self.squareDigits(n)

            if nextN == 1:
                return True
            elif nextN == n:
                return False
            elif nextN in intermediate:
                return False
            n = nextN
            intermediate.add(nextN)

    def squareDigits(self, n):
        res = 0
        for digit in str(n):
            digit = int(digit)
            res += digit * digit
        return res
```

# Floyd Cycle Detection Solution
```python
class Solution(object):
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        slow, fast = n, n
        while True:
            slow = self.squareDigits(slow)
            fast = self.squareDigits(fast)
            fast = self.squareDigits(fast)
            if slow == 1:
                return True
            if slow == fast:
                return False

    def squareDigits(self, n):
        res = 0
        for digit in str(n):
            digit = int(digit)
            res += digit * digit
        return res
```

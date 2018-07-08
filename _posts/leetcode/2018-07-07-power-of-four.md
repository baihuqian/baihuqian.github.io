---
layout: "post"
title: "Leetcode 342: Power of Four"
date: "2018-07-07 22:43"
tags:
  - Leetcode
---

# Question
Given an integer (signed 32 bits), write a function to check whether it is a power of 4.

Example:

Given num = 16, return true. Given num = 5, return false.

Follow up: Could you solve it without loops/recursion?

# Solution
```python
import re

class Solution(object):
    def isPowerOfFour(self, num):
        """
        :type num: int
        :rtype: bool
        """
        if num <= 0:
            return False
        elif num == 1:
            return True
        else:
            num = "{0:b}".format(num)
            return re.match("^10+$", num) is not None and len(num[1:]) % 2 == 0
```

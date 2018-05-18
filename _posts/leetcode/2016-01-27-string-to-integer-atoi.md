---
layout: "post"
title: "Leetcode 8: String to Integer (atoi)"
date: "2016-01-27 00:29"
tags: Leetcode
---

# Question
Implement atoi to convert a string to an integer.

Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.


# Solution

```python
class Solution:
    def myAtoi(self, str):
        """
        :type str: str
        :rtype: int
        """
        str = str.lstrip(' ')
        if str is None or len(str) == 0:
            return 0

        sign = 1
        res = 0
        if str[0] == '+':
            sign = 1
            str = str[1:]
        elif str[0] == '-':
            sign = -1
            str = str[1:]
        if len(str) == 0 or not str[0].isdigit():
            return 0

        for s in str:
            if s.isdigit():
                res = res * 10 + int(s)
            else:
                break

        res *= sign

        if res < -1 * 2 ** 31:
            return -1 * 2 ** 31
        elif res > 2 ** 31 - 1:
            return 2 ** 31 - 1
        else:
            return res
```

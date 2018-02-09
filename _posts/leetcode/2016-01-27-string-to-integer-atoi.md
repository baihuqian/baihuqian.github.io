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
import re

class Solution(object):
    def myAtoi(self, str):
        """
        :type str: str
        :rtype: int
        """
        str = str.lstrip()
        if len(str) == 0:
            return 0
        sign = 1
        if str[0] == '+':
            str = str[1:]
        elif str[0] == '-':
            str = str[1:]
            sign = -1
        i = 0
        while i < len(str) and str[i] == '0':
            i = i + 1
        str = str[i:]
        if len(str) == 0:
            return 0
        match = re.search(r'^[1-9]\d*', str)
        if match == None:
            return 0
        else:
            result = sign * int(match.group(0))

        if result > 2147483647:
            return 2147483647
        elif result < -2147483648:
            return -2147483648
        else:
            return result

```

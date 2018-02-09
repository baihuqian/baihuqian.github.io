---
layout: "post"
title: "Leetcode 13: Roman to Integer"
date: "2016-05-10 00:26"
tags: Leetcode
---

# Question
Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

# Solution

```python
class Solution(object):
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s) == 0:
            return 0
        elif len(s) == 1:
            return self.charToInt(s)
        chars = list(s)
        sum = 0
        previous = self.charToInt(chars[0])
        for i in range(1, len(chars)):
            next = self.charToInt(chars[i])
            if previous < next:
                sum -= previous
            else:
                sum += previous
            previous = next
        sum += previous
        return sum


    def charToInt(self, c):
        if c == 'I':
            return 1
        elif c == 'V':
            return 5
        elif c == 'X':
            return 10
        elif c == 'L':
            return 50
        elif c == 'C':
            return 100
        elif c == 'D':
            return 500
        elif c == 'M':
            return 1000
```

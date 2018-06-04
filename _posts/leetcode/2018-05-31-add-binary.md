---
layout: "post"
title: "Leetcode 67: Add Binary"
date: "2018-05-31 23:50"
tags:
  - Leetcode
---

# Question
Given two binary strings, return their sum (also a binary string).

The input strings are both non-empty and contains only characters `1` or `0`.

Example 1:
```
Input: a = "11", b = "1"
Output: "100"
```

Example 2:
```
Input: a = "1010", b = "1011"
Output: "10101"
```

# Solution
```python
class Solution:
    def addBinary(self, a, b):
        """
        :type a: str
        :type b: str
        :rtype: str
        """
        length = max(len(a), len(b))
        if len(a) < length:
            a = "0" * (length - len(a)) + a
        if len(b) < length:
            b = "0" * (length - len(b)) + b

        s = list()
        carry = 0
        for i in range(-1, -length - 1, -1):
            a_char = a[i]
            b_char = b[i]
            if a_char == "1" and b_char == "1":
                if carry == 0:
                    s.append("0")
                    carry = 1
                else:
                    s.append("1")
            elif a_char == "0" and b_char == "0":
                if carry == 1:
                    s.append("1")
                    carry = 0
                else:
                    s.append("0")
            else:
                if carry == 1:
                    s.append("0")
                else:
                    s.append("1")

        if carry == 1:
            s.append("1")
        s.reverse()
        return "".join(s)
```

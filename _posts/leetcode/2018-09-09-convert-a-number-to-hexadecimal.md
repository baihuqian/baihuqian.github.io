---
layout: "post"
title: "Leetcode 405: Convert a Number to Hexadecimal"
date: "2018-09-09 14:07"
tags:
  - Leetcode
---

# Question
Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

Note:

* All letters in hexadecimal (a-f) must be in lowercase.
* The hexadecimal string must not contain extra leading 0s. If the number is zero, it is represented by a single zero character '0'; otherwise, the first character in the hexadecimal string will not be the zero character.
* The given number is guaranteed to fit within the range of a 32-bit signed integer.
* You must not use any method provided by the library which converts/formats the number to hex directly.

Example 1:

```
Input:
26

Output:
"1a"
```

Example 2:

```
Input:
-1

Output:
"ffffffff"
```

# Solution
```python
class Solution:
    def toHex(self, num):
        """
        :type num: int
        :rtype: str
        """
        if num == 0:
            return '0'

        res = []
        if num > 0:
            while num > 0:
                res.append(num % 16)
                num >>= 4
        else:
            for i in range(8):
                res.append(num & 15)
                num >>= 4
        return ''.join(chr(d + ord('0')) if d <= 9 else chr(d - 10 + ord('a')) for d in res[::-1])
```

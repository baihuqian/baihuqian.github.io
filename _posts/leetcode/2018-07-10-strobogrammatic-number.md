---
layout: "post"
title: "Leetcode 246: Strobogrammatic Number"
date: "2018-07-10 22:16"
tags:
  - Leetcode
---

# Question
A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to determine if a number is strobogrammatic. The number is represented as a string.

Example 1:
```
Input:  "69"
Output: true
```

Example 2:
```
Input:  "88"
Output: true
```

Example 3:
```
Input:  "962"
Output: false
```

# Solution
```python
class Solution(object):
    def isStrobogrammatic(self, num):
        """
        :type num: str
        :rtype: bool
        """
        if len(num) == 0:
            return False

        for i in range((len(num) + 1) // 2):
            c1 = num[i]
            c2 = num[len(num) - 1 - i]
            if c1 == '8' == c2 == '8':
                pass
            elif c1 == '0' == c2:
                pass
            elif c1 == '1' == c2:
                pass
            elif (c1 == '6' and c2 == '9') or (c1 == '9' and c2 == '6'):
                pass
            else:
                return False

        return True
```

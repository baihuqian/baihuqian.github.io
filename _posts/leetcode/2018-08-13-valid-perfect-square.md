---
layout: "post"
title: "Leetcode 367: Valid Perfect Square"
date: "2018-08-13 21:46"
---

# Question
Given a positive integer num, write a function which returns True if num is a perfect square else False.

Note: Do not use any built-in library function such as `sqrt`.

Example 1:

```
Input: 16
Returns: True
```

Example 2:

```
Input: 14
Returns: False
```

# Solution
```python
class Solution:
    def isPerfectSquare(self, num):
        """
        :type num: int
        :rtype: bool
        """
        i = 1
        while True:
            if num == i * i:
                return True
            elif num < i * i:
                return False
            i += 1
```

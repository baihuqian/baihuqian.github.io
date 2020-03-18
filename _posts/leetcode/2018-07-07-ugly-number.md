---
layout: post
title: 'Leetcode 263: Ugly Number'
date: '2018-07-07 22:48'
tags:
  - Leetcode
published: true
---

# Question
Write a program to check whether a given number is an ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5.

Example 1:

```
Input: 6
Output: true
Explanation: 6 = 2 × 3
```

Example 2:

```
Input: 8
Output: true
Explanation: 8 = 2 × 2 × 2
```

Example 3:

```
Input: 14
Output: false
Explanation: 14 is not ugly since it includes another prime factor 7.
```

Note:

1. 1 is typically treated as an ugly number.
1. Input is within the 32-bit signed integer.

# Solution
```python
class Solution(object):
    def isUgly(self, num):
        """
        :type num: int
        :rtype: bool
        """
        if num <= 0:
            return False
        else:
            while num != 1:
                if num % 2 == 0:
                    num //= 2
                elif num % 3 == 0:
                    num //= 3
                elif num % 5 == 0:
                    num //= 5
                else:
                    return False

            return True
```

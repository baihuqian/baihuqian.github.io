---
layout: "post"
title: "Leetcode 7: Reverse Integer"
date: "2018-05-17 22:26"
tags: Leetcode
---

# Question

Given a 32-bit signed integer, reverse digits of an integer.

Example 1:
```
Input: 123
Output: 321
```

Example 2:
```
Input: -123
Output: -321
```
Example 3:
```
Input: 120
Output: 21
```
Note:

Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $$[−2^{31},  2^{31} − 1]$$. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.


# Solution
```python
class Solution:
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        if x == 0:
            return x
        elif x > 0:
            sign = 1
        else:
            sign = -1
            x = -1 * x

        res = 0
        while x != 0:
            digit = x % 10
            x //= 10
            res = res * 10 + digit

        res *= sign

        if -1 * 2 ** 31 <= res <= 2 ** 31 - 1:
            return res
        else:
            return 0
```

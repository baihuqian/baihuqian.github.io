---
layout: "post"
title: "Leetcode 191: Number of 1 Bits"
date: "2018-07-03 20:35"
tags:
  - Leetcode
---

# Question
Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).

Example 1:
```
Input: 11
Output: 3
Explanation: Integer 11 has binary representation 00000000000000000000000000001011
```

Example 2:
```
Input: 128
Output: 1
Explanation: Integer 128 has binary representation 00000000000000000000000010000000
```

# Solution
```python
class Solution(object):
    def hammingWeight(self, n):
        """
        :type n: int
        :rtype: int
        """
        count = 0
        while n != 0:
            if n % 2 == 1:
                count += 1
            n //= 2

        return count
```

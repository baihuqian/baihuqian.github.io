---
layout: "post"
title: "Leetcode 201: Bitwise AND of Numbers Range"
date: "2018-07-03 21:45"
tags:
  - Leetcode
---

# Question
Given a range `[m, n]` where `0 <= m <= n <= 2147483647`, return the bitwise AND of all numbers in this range, inclusive.

Example 1:
```
Input: [5,7]
Output: 4
```

Example 2:
```
Input: [0,1]
Output: 0
```

# Solution
One bit can be set if and only if both m and n have this bit set, and the range is not larger than the number of this bit.

```python
class Solution(object):
    def rangeBitwiseAnd(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        res = 0
        bit = 1
        diff = n - m + 1

        while m >= bit:
            if m & bit != 0 and n & bit != 0 and diff <= bit:
                res += bit
            bit *= 2

        return res
```

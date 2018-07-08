---
layout: "post"
title: "Leetcode 231: Power of Two"
date: "2018-07-07 22:25"
tags:
  - Leetcode
---

# Question
Given an integer, write a function to determine if it is a power of two.

Example 1:
```
Input: 1
Output: true
Explanation: 2^0 = 1
```

Example 2:
```
Input: 16
Output: true
Explanation: 2^4 = 16
```

Example 3:

```
Input: 218
Output: false
```

# Solution
There are 4 ways of doing this:
1. keep dividing the number with 2 until it becomes 1;
2. convert the number to base-2 integer and check only the first digit is 1;
3. if the range if input is bounded, we can find the largest power of two in the range (say, `max_power_2`), and `max_power_2 % n == 0` is true.

```python
class Solution(object):
    def isPowerOfTwo(self, n):
        """
        :type n: int
        :rtype: bool
        """
        if n <= 0:
            return False
        elif n == 1:
            return True
        else:
            while n != 1:
                if n % 2 == 0:
                    n //= 2
                else:
                    return False
            return True
```

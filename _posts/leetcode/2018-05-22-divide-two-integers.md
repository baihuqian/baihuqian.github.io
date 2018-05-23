---
layout: "post"
title: "Leetcode 29: Divide Two Integers"
date: "2018-05-22 22:30"
tags:
  - Leetcode
---

# Question
Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero.

Example 1:
```
Input: dividend = 10, divisor = 3
Output: 3
```

Example 2:
```
Input: dividend = 7, divisor = -3
Output: -2
```

Note:

* Both dividend and divisor will be 32-bit signed integers.
* The divisor will never be 0.
* Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $$[−2^{31},  2^{31} − 1]$$. For the purpose of this problem, assume that your function returns $$2^{31} − 1$$ when the division result overflows.

# Solution
```python
class Solution:
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        if dividend == 0:
            return 0

        if (dividend > 0 and divisor > 0) or (dividend < 0 and divisor < 0):
            sign = 1
        else:
            sign = -1

        if dividend < 0:
            dividend = 0 - dividend
        if divisor < 0:
            divisor = 0 - divisor

        if dividend < divisor:
            return 0

        s_dividend = str(dividend)
        n = len(s_dividend)
        m = len(str(divisor))
        result = []
        residue = 0


        for i in range(0, n-m+1):
            digit = 0
            if i == 0:
                num = int(s_dividend[0:m])
            else:
                num = int(str(residue) + s_dividend[m-1+i])
            while num >= divisor:
                num -= divisor
                digit += 1
            residue = num
            result.append(str(digit))

        res = int(''.join(result)) if sign == 1 else int('-'+''.join(result))
        if res > 2147483647:
            res = 2147483647
        if res < -2147483648:
            res = -2147483648
        return res
```

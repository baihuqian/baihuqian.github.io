---
layout: "post"
title: "Leetcode 43: Multiply Strings"
date: "2018-06-03 21:11"
---

# Question
Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

Example 1:
```
Input: num1 = "2", num2 = "3"
Output: "6"
```

Example 2:
```
Input: num1 = "123", num2 = "456"
Output: "56088"
```

Note:

1. The length of both num1 and num2 is < 110.
1. Both num1 and num2 contain only digits 0-9.
1. Both num1 and num2 do not contain any leading zero, except the number 0 itself.
1. You must not use any built-in BigInteger library or convert the inputs to integer directly.

# Solution
```python
class Solution:
    def multiply(self, num1, num2):
        """
        :type num1: str
        :type num2: str
        :rtype: str
        """
        if num1 == "0" or num2 == "0":
            return "0"
        num1 = list(num1)
        num2 = list(num2)
        num1.reverse()
        num2.reverse()
        factor = 1
        total_sum = [0] * (len(num2) + len(num1))
        for i in range(len(num2)):
            for j in range(len(num1)):
                position = i + j
                product = int(num1[j]) * int(num2[i]) + total_sum[position]
                digit = product % 10
                carry = product // 10
                total_sum[position] = digit
                while carry > 0:
                    position += 1
                    product = carry + total_sum[position]
                    digit = product % 10
                    carry = product // 10
                    total_sum[position] = digit

        while total_sum[-1] == 0:
            total_sum.pop()

        total_sum.reverse()
        return "".join([str(x) for x in total_sum])  
```

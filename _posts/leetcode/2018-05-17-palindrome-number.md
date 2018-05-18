---
layout: "post"
title: "Leetcode 9: Palindrome Number"
date: "2018-05-17 23:20"
tags: Leetcode
---

# Question
Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

Example 1:
```
Input: 121
Output: true
```

Example 2:

```
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```
Example 3:
```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

# Solution
Use String:

```python
class Solution:
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        s = str(x)
        if len(s) % 2 != 0:
            left, right = len(s) // 2, len(s) // 2
        else:
            left, right = len(s) // 2 - 1, len(s) // 2

        while left >= 0:
            if s[left] != s[right]:
                return False
            else:
                left -= 1
                right += 1

        return True
```

Without string conversion:

```python
import math

class Solution:
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        if 0 <= x <= 9:
            return True
        elif x < 0:
            return False

        digits = math.floor(math.log(x, 10)) + 1
        stack = list()

        for count in range(0, digits):
            digit = x % 10
            x //= 10
            if count < digits // 2:
                stack.append(digit)
            elif count == digits // 2:
                if digits % 2 == 0:
                    matching_digit = stack.pop()
                    if digit != matching_digit:
                        return False
            else:
                matching_digit = stack.pop()
                if digit != matching_digit:
                    return False

        return True
```

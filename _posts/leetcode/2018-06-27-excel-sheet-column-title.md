---
layout: "post"
title: "Leetcode 168: Excel Sheet Column Title"
date: "2018-06-27 22:00"
tags:
  - Leetcode
---

# Question
Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:
```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB
    ...
```

Example 1:
```
Input: 1
Output: "A"
```

Example 2:
```
Input: 28
Output: "AB"
```

Example 3:
```
Input: 701
Output: "ZY"
```

# Solution
There is no 0 in Excel's representation. When a digit is 26, it is replaced by 26 ('Z').

```python
class Solution(object):
    def convertToTitle(self, n):
        """
        :type n: int
        :rtype: str
        """
        if n <= 0:
            return ""

        A = ord('A')
        res = []
        while n > 0:
            if n > 26:
                i = n % 26
                if i == 0:
                    res.append('Z')
                    n -= 26
                else:
                    res.append(chr(A + i - 1))
                n //= 26
            else:
                res.append(chr(A + n - 1))
                break
        res.reverse()
        return ''.join(res)
```

---
layout: "post"
title: "Leetcode 171: Excel Sheet Column Number"
date: "2016-02-15 00:08"
tags: Leetcode
---

# Question
Related to question [Excel Sheet Column Title](https://leetcode.com/problems/excel-sheet-column-title/)

Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:
```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28
```

# Solution

```python
class Solution(object):
    def titleToNumber(self, s):
        """
        :type s: str
        :rtype: int
        """
        sum = 0
        if len(s) == 0:
            return 0
        for i in range(0, len(s)):
            d = ord(s[i]) - ord('A') + 1
            sum = sum * 26 + d
        return sum
```

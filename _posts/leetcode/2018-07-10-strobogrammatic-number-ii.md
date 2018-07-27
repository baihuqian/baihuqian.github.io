---
layout: "post"
title: "Leetcode 247: Strobogrammatic Number II"
date: "2018-07-10 22:27"
tags:
  - Leetcode
---

# Question
A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of length = n.

Example:
```
Input:  n = 2
Output: ["11","69","88","96"]
```

# Solution
```python
class Solution(object):
    def findStrobogrammatic(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        if n <= 0:
            return []

        left = []
        right = []
        for i in range(n // 2):
            if i == 0:
                left = ['1', '8', '9', '6']
                right = ['1', '8', '6', '9']
            else:
                new_left = []
                new_right = []
                for l in left:
                    for c in ['1', '0', '8', '9', '6']:
                        new_left.append(l + c)
                for r in right:
                    for c in ['1', '0', '8', '6', '9']:
                        new_right.append(c + r)
                left = new_left
                right = new_right

        res = []
        if n % 2 == 1:
            for m in ['1', '0', '8']:
                if len(left) > 0:
                    for i in range(len(left)):
                        res.append(left[i] + m + right[i])
                else:
                    res.append(m)
        else:
            for i in range(len(left)):
                res.append(left[i] + right[i])

        return res
```

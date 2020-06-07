---
layout: "post"
title: "Leetcode 119: Pascal's Triangle II"
date: "2018-06-20 20:10"
tags:
  - Leetcode
---

# Question
Given a non-negative index k where k ≤ 33, return the $$k^{th}$$ index row of the Pascal's triangle.

Note that the row index starts from 0.

![Pascal Triangle](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

Example:
```
Input: 3
Output: [1,3,3,1]
```

Follow up:

Could you optimize your algorithm to use only O(k) extra space?

# Solution
```python
class Solution(object):
    def getRow(self, rowIndex):
        """
        :type rowIndex: int
        :rtype: List[int]
        """
        res = [1 for _ in range(rowIndex // 2 + 1)]

        for j in range(1, rowIndex + 1):
            for i in range(j // 2, 0, -1):
                if j % 2 == 0 and i == j // 2:
                    res[i] = 2 * res[i - 1]
                else:
                    res[i] += res[i - 1]

        start = len(res) - 1 if rowIndex % 2 == 1 else len(res) - 2
        for i in range(start, -1, -1):
            res.append(res[i])
        return res
```

---
layout: post
title: 'Leetcode 59: Spiral Matrix II'
date: '2018-06-07 22:04'
tags:
  - Leetcode
---

# Question
Given a positive integer n, generate a square matrix filled with elements from 1 to n2 in spiral order.

Example:

```
Input: 3
Output:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```

# Solution
```python
class Solution:
    def generateMatrix(self, n):
        """
        :type n: int
        :rtype: List[List[int]]
        """
        if n == 0:
            return []

        res = [[0 for _ in range(n)] for _ in range(n)]

        top, bottom, left, right = -1, n, -1, n
        i, j = 0, 0
        direction = 0

        for idx in range(1, n * n + 1):
            res[i][j] = idx
            if direction == 0:
                if j < right - 1:
                    j += 1
                else:
                    i += 1
                    top += 1
                    direction = 1
            elif direction == 1:
                if i < bottom - 1:
                    i += 1
                else:
                    j -= 1
                    right -= 1
                    direction = 2
            elif direction == 2:
                if j > left + 1:
                    j -= 1
                else:
                    i -= 1
                    bottom -= 1
                    direction = 3
            else:
                if i > top + 1:
                    i -= 1
                else:
                    j += 1
                    left += 1
                    direction = 0

        return res
```

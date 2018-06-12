---
layout: post
title: 'Leetcode 54: Spiral Matrix'
date: '2018-06-07 21:58'
tags:
  - Leetcode
---

# Question
Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

Example 1:
```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```

Example 2:
```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

# Solution
```python
class Solution:
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return []

        top, bottom, left, right = -1, len(matrix), -1, len(matrix[0])

        direction = 0 # 0: right, 1: down, 2: left, 3: up
        i, j = 0, 0
        res = []
        for idx in range(bottom * right):
            res.append(matrix[i][j])
            if direction == 0:
                j += 1
                if j == right:
                    direction = 1
                    top += 1
                    j -= 1
                    i += 1
            elif direction == 1:
                i += 1
                if i == bottom:
                    direction = 2
                    right -= 1
                    i -= 1
                    j -= 1
            elif direction == 2:
                j -= 1
                if j == left:
                    direction = 3
                    bottom -= 1
                    j += 1
                    i -= 1
            else:
                i -= 1
                if i == top:
                    direction = 0
                    left += 1
                    i += 1
                    j += 1

        return res
        
```

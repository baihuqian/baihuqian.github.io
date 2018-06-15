---
layout: post
title: 'Leetcode 74: Search a 2D Matrix'
date: '2018-06-13 21:25'
tags:
  - Leetcode
---

# Question
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

Example 1:
```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true
```

Example 2:
```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

# Solution
```python
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return False
        if target < matrix[0][0] or target > matrix[-1][-1]:
            return False

        x, y = len(matrix), len(matrix[0])

        top, bottom = 0, x
        while bottom - top > 1:
            mid = (top + bottom) // 2
            if target > matrix[mid][0]:
                top = mid
            elif target < matrix[mid][0]:
                bottom = mid
            else:
                return True

        row = top
        left, right = 0, y
        while right - left > 1:
            mid = (left + right) // 2
            if target > matrix[row][mid]:
                left = mid
            elif target < matrix[row][mid]:
                right = mid
            else:
                return True

        return target == matrix[row][left]
         
```

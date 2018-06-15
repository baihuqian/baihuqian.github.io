---
layout: "post"
title: "Leetcode 240: Search a 2D Matrix II"
date: "2018-06-13 22:26"
tags:
  - Leetcode
---

# Question
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

* Integers in each row are sorted in ascending from left to right.
* Integers in each column are sorted in ascending from top to bottom.

Example:
```
Consider the following matrix:

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = 5, return true.

Given target = 20, return false.

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
        elif target == matrix[0][0] or target == matrix[-1][-1]:
            return True

        Y, X = len(matrix), len(matrix[0])
        x, y = X, Y
        top, bottom, left, right = 0, y - 1, 0, x - 1

        while bottom - top > 1 or right - left > 1:
            x_mid = (left + right) // 2
            y_mid = (top + bottom) // 2
            mid_val = matrix[y_mid][x_mid]
            if target > mid_val:
                left = x_mid
                top = y_mid
            elif target < mid_val:
                right = x_mid
                bottom = y_mid
            else:
                return True

        x, y = left, top

        for i in range(y + 1, Y):
            if target == matrix[i][0] or target == matrix[i][-1]:
                return True
            elif matrix[i][0] < target < matrix[i][-1]:
                left, right = 0, X - 1
                while right - left > 1:
                    mid = (left + right) // 2
                    mid_val = matrix[i][mid]
                    if target > mid_val:
                        left = mid
                    elif target < mid_val:
                        right = mid
                    else:
                        return True

        for j in range(x + 1, X):
            if target == matrix[0][j] or target == matrix[y][j]:
                return True
            elif matrix[0][j] < target < matrix[y][j]:
                top, bottom = 0, y
                while bottom - top > 1:
                    mid = (top + bottom) // 2
                    mid_val = matrix[mid][j]
                    if target > mid_val:
                        top = mid
                    elif target < mid_val:
                        bottom = mid
                    else:
                        return True

        return False

```

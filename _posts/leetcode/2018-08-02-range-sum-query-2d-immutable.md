---
layout: "post"
title: "Leetcode 304: Range Sum Query 2D - Immutable"
date: "2018-08-02 22:13"
tags:
  - Leetcode
---

# Question
Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

![Range Sum Query 2D](https://leetcode.com/static/images/courses/range_sum_query_2d.png)

The above rectangle (with the red border) is defined by (row1, col1) = (2, 1) and (row2, col2) = (4, 3), which contains sum = 8.

Example:

```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```

Note:
1. You may assume that the matrix does not change.
1. There are many calls to sumRegion function.
1. You may assume that row1 ≤ row2 and col1 ≤ col2.

# Solution
```python
class NumMatrix(object):

    def __init__(self, matrix):
        """
        :type matrix: List[List[int]]
        """
        if len(matrix) == 0 or len(matrix[0]) == 0:
            self.X, self.Y = 0, 0
            self.sum = [[]]
            return

        self.X = len(matrix)
        self.Y = len(matrix[0])
        self.sum = list()
        self.sum.append([matrix[0][0]])
        for i in range(1, self.Y):
            self.sum[0].append(self.sum[0][-1] + matrix[0][i])

        for i in range(1, self.X):
            prev = matrix[i][0]
            self.sum.append([self.sum[i - 1][0] + matrix[i][0]])
            for j in range(1, self.Y):
                prev += matrix[i][j]
                self.sum[i].append(self.sum[i - 1][j] + prev)


    def sumRegion(self, row1, col1, row2, col2):
        """
        :type row1: int
        :type col1: int
        :type row2: int
        :type col2: int
        :rtype: int
        """
        return self.sum[row2][col2] - \
          (self.sum[row2][col1 - 1] if col1 > 0 else 0) - \
          (self.sum[row1 - 1][col2] if row1 > 0 else 0) + \
          (self.sum[row1 - 1][col1 - 1] if col1 > 0 and row1 > 0 else 0)



# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# param_1 = obj.sumRegion(row1,col1,row2,col2)
```

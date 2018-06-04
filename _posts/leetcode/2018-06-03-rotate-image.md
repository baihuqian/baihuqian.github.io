---
layout: post
title: "Leetcode 48: Rotate Image"
date: '2018-06-03 15:51'
tags:
  - Leetcode
---

# Question
You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

Note:

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

Example 1:

```
Given input matrix =
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

Example 2:

```
Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
],

rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

# Solution
```python
class Solution:
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: void Do not return anything, modify matrix in-place instead.
        """
        length = len(matrix)
        if length <= 1:
            return
        half_i = length // 2 if length % 2 == 0 else length // 2 + 1
        half_j = length // 2
        length -= 1

        for i in range(0, half_i):
            for j in range(0, half_j):
                # (i, j) -> (j, len-i) -> (len-i, len-j) -> (len-j, i) -> (i,j)
                temp1 = matrix[i][j]
                matrix[i][j] = matrix[length-j][i]
                temp2 = matrix[j][length-i]
                matrix[j][length-i] = temp1
                temp1 = matrix[length-i][length-j]
                matrix[length-i][length-j] = temp2
                matrix[length-j][i] = temp1
```

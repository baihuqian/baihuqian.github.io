---
layout: "post"
title: "Leetcode 311: Sparse Matrix Multiplication"
date: "2018-08-02 22:59"
tags:
  - Leetcode
  - Review
---

# Question
Given two sparse matrices A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

Example:

```
Input:

A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]

Output:

     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
```

# Solution
Note:

`[[0 for _ in range(b)] for _ in range(a)]` generates a b-by-a matrix, while `[[0] * b] * a` will cause `a` rows to be referenced to the same list.

```python
class Solution(object):
    def multiply(self, A, B):
        """
        :type A: List[List[int]]
        :type B: List[List[int]]
        :rtype: List[List[int]]
        """
        if len(A) == 0 or len(B) == 0:
            return [[]]

        a, c, b = len(A), len(B), len(B[0])
        AB = [[0 for _ in range(b)] for _ in range(a)]

        for i in range(a):
            for j in range(c):
                if A[i][j] != 0:
                    for k in range(b):
                        if B[j][k] != 0:
                            AB[i][k] += A[i][j] * B[j][k]

        return AB
```

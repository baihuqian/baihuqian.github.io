---
layout: "post"
title: "Leetcode 52: N-Queens II"
date: "2018-07-28 21:52"
tags:
  - Leetcode
  - Backtracking
  - Hard
---

# Question
The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

[https://leetcode.com/static/images/problemset/8-queens.png](N-Queen)

Given an integer n, return the number of distinct solutions to the n-queens puzzle.

Example:

```
Input: 4
Output: 2
Explanation: There are two distinct solutions to the 4-queens puzzle as shown below.
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

# Solution
Use backtracking. See N-Queen for explanation.
```python
class Solution(object):
    def totalNQueens(self, n):
        """
        :type n: int
        :rtype: int
        """
        self.col = [True] * n
        self.diag1 = [True] * (2 * n - 1)
        self.diag2 = [True] * (2 * n - 1)
        self.queens = [-1] * n
        self.n = n

        return self.__solve(0)

    def __solve(self, row): # backtracking
        if row == self.n:
            return 1
        else:
            res = 0
            for col in range(self.n):
                if self.__isAvailable(row, col):
                    self.__place(row, col)
                    res += self.__solve(row + 1)
                    self.__remove(row, col)
            return res


    def __isAvailable(self, row, col):
        return self.col[col] and self.diag1[row + col] and self.diag2[row - col + self.n - 1]

    def __place(self, row, col):
        self.queens[row] = col
        self.col[col] = False
        self.diag1[row + col] = False
        self.diag2[row - col + self.n - 1] = False

    def __remove(self, row, col):
        self.queens[row] = -1
        self.col[col] = True
        self.diag1[row + col] = True
        self.diag2[row - col + self.n - 1] = True
```

---
layout: "post"
title: "Leetcode 36: Valid Sudoku"
date: "2018-05-23 22:01"
tags:
  - Leetcode
---

# Question
Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

1. Each row must contain the digits 1-9 without repetition.
1. Each column must contain the digits 1-9 without repetition.
1. Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.

Example 1:
```
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true
```

Example 2:
```
Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false

Explanation: Same as Example 1, except with the 5 in the top left corner being
    modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```
Note:

* A Sudoku board (partially filled) could be valid but is not necessarily solvable.
* Only the filled cells need to be validated according to the mentioned rules.
* The given board contain only digits 1-9 and the character '.'.
* The given board size is always 9x9.

# Solution
```python
class Solution:
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """
        for l in board:
            if self.validate(l) == False:
                return False

        for i in range(0, 9):
            if self.validate([l[i] for l in board]) == False:
                return False

        for i in range(0, 3):
            for j in range(0, 3):
                l = list()
                for k in range(0, 3):
                    l.extend(board[j * 3 + k][i*3:(i+1)*3])
                if self.validate(l) == False:
                    return False

        return True

    def validate(self, block):
        num_set = set()
        for n in block:
            if n != ".":
                num = int(n)
                if 1 <= num <= 9 and num not in num_set:
                    num_set.add(num)
                else:
                    return False

        return True
```

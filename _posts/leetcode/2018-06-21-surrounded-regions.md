---
layout: "post"
title: "Leetcode 130: Surrounded Regions"
date: "2018-06-21 20:51"
tags:
  - Leetcode
---

Given a 2D board containing 'X' and 'O' (the letter O), capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

Example:
```
X X X X
X O O X
X X O X
X O X X
```

After running your function, the board should be:
```
X X X X
X X X X
X X X X
X O X X
```

Explanation:

Surrounded regions shouldn’t be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.

# Solution
```python
class Solution(object):
    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: void Do not return anything, modify board in-place instead.
        """
        if len(board) == 0 or len(board[0]) == 0:
            return

        zeros = set()
        border_zeros = list()

        x, y = len(board), len(board[0])
        for i in range(x):
            for j in range(y):
                if board[i][j] == 'O':
                    if i == 0 or i == x - 1 or j == 0 or j == y - 1:
                        border_zeros.append((i, j))
                    else:
                        zeros.add((i, j))

        for i, j in border_zeros:
            self.walk_border_zeros(zeros, i, j)

        for i, j in zeros:
            board[i][j] = 'X'

    def walk_border_zeros(self, zeros, i, j):
        if (i + 1, j) in zeros:
            zeros.remove((i + 1, j))
            self.walk_border_zeros(zeros, i + 1, j)
        if (i - 1, j) in zeros:
            zeros.remove((i - 1, j))
            self.walk_border_zeros(zeros, i - 1, j)
        if (i, j + 1) in zeros:
            zeros.remove((i, j + 1))
            self.walk_border_zeros(zeros, i, j + 1)
        if (i, j - 1) in zeros:
            zeros.remove((i, j - 1))
            self.walk_border_zeros(zeros, i, j - 1)

```

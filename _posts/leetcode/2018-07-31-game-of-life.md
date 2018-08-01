---
layout: "post"
title: "Leetcode 289: Game of Life"
date: "2018-07-31 20:30"
tags:
  - Leetcode
---

# Question
According to the Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
1. Any live cell with two or three live neighbors lives on to the next generation.
1. Any live cell with more than three live neighbors dies, as if by over-population..
1. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.

Example:

```
Input:
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
Output:
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

Follow up:

* Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
* In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

# Solution
```python
class Solution(object):
    def gameOfLife(self, board):
        """
        :type board: List[List[int]]
        :rtype: void Do not return anything, modify board in-place instead.
        """
        lives = set()
        for i in range(len(board)):
            for j in range(len(board[0])):
                if board[i][j] == 1:
                    lives.add((i, j))

        for (i, j) in lives:
            if not self.checkNeighbors(False, i, j, lives):
                board[i][j] = 0

        for i in range(len(board)):
            for j in range(len(board[0])):
                if (i, j) not in lives and self.checkNeighbors(True, i, j, lives):
                    board[i][j] = 1

    def checkNeighbors(self, isDead, i, j, lives):
        live_neighbor = 0
        if (i + 1, j) in lives:
            live_neighbor += 1
        if (i - 1, j) in lives:
            live_neighbor += 1
        if (i, j + 1) in lives:
            live_neighbor += 1
        if (i, j - 1) in lives:
            live_neighbor += 1
        if (i + 1, j + 1) in lives:
            live_neighbor += 1
        if (i + 1, j - 1) in lives:
            live_neighbor += 1
        if (i - 1, j + 1) in lives:
            live_neighbor += 1
        if (i - 1, j - 1) in lives:
            live_neighbor += 1

        if isDead:
            return live_neighbor == 3
        else:
            return 2 <= live_neighbor <= 3
```

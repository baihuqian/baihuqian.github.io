---
layout: post
title: 'Leetcode 79: Word Search'
date: '2018-06-14 22:02'
tags:
  - Leetcode
  - Backtracking
---

# Question
Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

Example:
```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

# Solution
```python
class Solution:
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        if len(word) == 0:
            return True
        if len(board) == 0 or len(board[0]) == 0:
            return False
        x, y = len(board), len(board[0])

        char = word[0]
        for i in range(x):
            for j in range(y):
                if board[i][j] == char:
                    ret = self.backtracking_one_step(board, x, y, word, 0, i, j, char)
                    if ret:
                        return True

        return False

    def backtracking(self, board, x, y, word, index, i, j):
        if index == len(word) - 1:
            return True

        index += 1
        char = word[index]
        if i > 0 and board[i - 1][j] == char:
            if self.backtracking_one_step(board, x, y, word, index, i - 1, j, char):
                return True
        if i < x - 1 and board[i + 1][j] == char:
            if self.backtracking_one_step(board, x, y, word, index, i + 1, j, char):
                return True
        if j > 0 and board[i][j - 1] == char:
            if self.backtracking_one_step(board, x, y, word, index, i, j - 1, char):
                return True
        if j < y - 1 and board[i][j + 1] == char:
            if self.backtracking_one_step(board, x, y, word, index, i, j + 1, char):
                return True

        return False

    def backtracking_one_step(self, board, x, y, word, index, i, j, char):
        board[i][j] = ''
        ret = self.backtracking(board, x, y, word, index, i, j)
        if ret:
            return True
        else:
            board[i][j] = char
            return False
```

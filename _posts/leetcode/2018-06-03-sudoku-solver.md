---
layout: post
title: 'Leetcode 37: Sudoku Solver'
date: '2018-06-03 15:18'
tags:
  - Leetcode
  - Backtracking
  - Hard
  - Review
---

# Question
Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:

1. Each of the digits 1-9 must occur exactly once in each row.
1. Each of the digits 1-9 must occur exactly once in each column.
1. Each of the the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.

Empty cells are indicated by the character '.'.

Note:

* The given board contain only digits 1-9 and the character '.'.
* You may assume that the given Sudoku puzzle will have a single unique solution.
* The given board size is always 9x9.

# Solution
There are 27 constraints: 9 rows, 9 columns, and 9 sub-boxes. Use a set for each constraint to represent possible values so far. Use backtracking to try each number. Before backtracking, iterate through positions where only one possible option can be found.

```python
class Solution:
    def solveSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: void Do not return anything, modify board in-place instead.
        """
        # Populate unused values for each constraint
        # 0-8 is row, 9-17 is column, and 18-26 is sub-box
        options = [set(range(1, 10)) for i in range(27)]
        pos = [] # Empty positions

        for i in range(0, 9):
            for j in range(0, 9):
                c = board[i][j]
                if c != '.':
                    num = int(c)
                    options[i].remove(num)
                    options[9 + j].remove(num)
                    options[18 + (i//3)*3 + j//3].remove(num)
                else:
                    pos.append((i, j))

        self.populateSingleOption(pos, options, board)
        answer = []
        self.backtracking(pos, answer, options)
        for k in range(0, len(pos)):
            i, j = pos[k]
            num = answer[k]
            board[i][j] = str(num)

        return

    def populateSingleOption(self, pos, options, board):
        min_options = 1 # Track whether there are single options populated in last iteration
        while min_options == 1:
            min_options = 10 # max value
            for k in range(len(pos) - 1, -1, -1): # Because items are removed during iteration, do so from the the end
                i, j = pos[k]
                option = set.intersection(options[i], options[9+j], options[18 + (i//3)*3 + j//3])
                if not option:
                    return
                if len(option) < min_options:
                    min_options = len(option)
                if len(option) == 1:
                    num = option.pop()
                    board[i][j] = str(num)
                    pos.pop(k) # This position is populated, remove
                    options[i].remove(num)
                    options[9+j].remove(num)
                    options[18 + (i//3)*3 + j//3].remove(num)

    def backtracking(self, pos, answer, options):
        if len(answer) == len(pos):
            return

        i, j = pos[len(answer)]
        option = set.intersection(options[i], options[9+j], options[18 + (i//3)*3 + j//3])

        if len(option) == 0:
            return
        else:
            for num in option:
                # Try this number
                answer.append(num)
                options[i].remove(num)
                options[9+j].remove(num)
                options[18 + (i//3)*3 + j//3].remove(num)

                self.backtracking(pos, answer, options)
                if len(answer) == len(pos):
                    break

                # This number does not satisfy requirement, restore to previous state
                answer.pop()
                options[i].add(num)
                options[9+j].add(num)
                options[18 + (i//3)*3 + j//3].add(num)

```

---
layout: post
title: 'Leetcode 361: Bomb Enemy'
date: '2018-08-12 20:40'
tags:
  - Leetcode
published: true
---

# Question
Given a 2D grid, each cell is either a wall `'W'`, an enemy `'E'` or empty `'0'` (the number zero), return the maximum enemies you can kill using one bomb.

The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.

Note: You can only put the bomb at an empty cell.

Example:
```
Input: [["0","E","0","0"],["E","0","W","E"],["0","E","0","0"]]
Output: 3
Explanation: For the given grid,

0 E 0 0
E 0 W E
0 E 0 0

Placing a bomb at (1,1) kills 3 enemies.
```

# Solution
```python
class Solution:
    def maxKilledEnemies(self, grid):
        """
        :type grid: List[List[str]]
        :rtype: int
        """
        if len(grid) == 0 or len(grid[0]) == 0:
            return 0
        x, y = len(grid), len(grid[0])

        empty = list()
        count = 0
        for i in range(x):
            for j in range(y + 1): # add one to process the right-most portion
                if j == y or grid[i][j] == "W":
                    if count > 0:
                        for jj in empty:
                            grid[i][jj] += count
                    empty = list()
                    count = 0
                elif grid[i][j] == "0":
                    empty.append(j)
                    grid[i][j] = 0
                elif grid[i][j] == "E":
                    count += 1

        empty = list()
        count = 0
        for j in range(y):
            for i in range(x + 1):
                if i == x or grid[i][j] == "W":
                    if count > 0:
                        for ii in empty:
                            grid[ii][j] += count
                    empty = list()
                    count = 0
                elif grid[i][j] == "E":
                    count += 1
                else:
                    empty.append(i)
        res = 0
        for i in range(x):
            for j in range(y):
                if isinstance(grid[i][j], int):
                    res = max(res, grid[i][j])

        return res
```

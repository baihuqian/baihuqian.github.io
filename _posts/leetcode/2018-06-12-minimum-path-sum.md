---
layout: post
title: 'Leetcode 64: Minimum Path Sum'
date: '2018-06-12 22:55'
tags:
  - Leetcode
  - DP
---

# Question
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

Example:
```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```

# Solution
```python
class Solution:
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        if len(grid) == 0 or len(grid[0]) == 0:
            return 0

        x, y = len(grid), len(grid[0])

        sums = [0 for _ in range(y)]

        for i in range(x-1, -1, -1):
            for j in range(y-1, -1, -1):
                if i == x - 1:
                    if j == y - 1:
                        sums[j] = grid[i][j]
                    else:
                        sums[j] = sums[j + 1] + grid[i][j]
                else:
                    if j == y - 1:
                        sums[j] += grid[i][j]
                    else:
                        sums[j] = min(sums[j + 1], sums[j]) + grid[i][j]

        return sums[0]



```

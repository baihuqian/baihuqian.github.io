---
layout: post
title: 'Leetcode 63: Unique Paths II'
date: '2018-06-07 22:41'
tags:
  - Leetcode
  - DP
---

# Question
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

Note: m and n will be at most 100.

Example 1:
```
Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

# Solution
Use dynamic programming, scan from top-left to bottom-right. number of paths to `(i,j)` is 0 if it is an obstacle, or number of paths to its top plus number of paths to its left.

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid):
        """
        :type obstacleGrid: List[List[int]]
        :rtype: int
        """
        if len(obstacleGrid) == 0 or len(obstacleGrid[0]) == 0:
            return 0
        if obstacleGrid[0][0] == 1:
            return 0

        m, n = len(obstacleGrid), len(obstacleGrid[0])

        dp = [0 for _ in range(n)]
        dp[0] = 1

        for row in obstacleGrid:
            for j in range(n):
                if row[j] == 1:
                    dp[j] = 0
                elif j > 0:
                    #dp[j] is number of paths coming from the top
                    #dp[j - 1] is the number of paths coming from the left
                    dp[j] += dp[j - 1]

        return dp[-1]
```

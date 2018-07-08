---
layout: post
title: 'Leetcode 221: Maximal Square'
date: '2018-07-07 22:18'
tags:
  - Leetcode
  - DP
  - Review
---

# Question
Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

Example:

```
Input:

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

Output: 4
```

# Solution
We initialize another matrix (dp) with the same dimensions as the original one initialized with all 0’s.

`dp(i,j)` represents the side length of the maximum square whose bottom right corner is the cell with index (i,j) in the original matrix.

Starting from index (0,0), for every 1 found in the original matrix, we update the value of the current element as

$$\text{dp}(i,\ j) = \min \big( \text{dp}(i-1,\ j),\ \text{dp}(i-1,\ j-1),\ \text{dp}(i,\ j-1) \big) + 1$$.

We also remember the size of the largest square found so far. In this way, we traverse the original matrix once and find out the required maximum size. This gives the side length of the square (say $$maxsqlen$$). The required result is the area $$maxsqlen^2$$.

In the previous approach for calculating dp of $$i^{th}$$ row we are using only the previous element and the $$(i-1)^{th}$$ row. Therefore, we don't need 2D dp matrix as 1D dp array will be sufficient for this.

Initially the dp array contains all 0's. As we scan the elements of the original matrix across a row, we keep on updating the dp array as per the equation $$dp[j]=min(dp[j-1],dp[j],prev)$$, where $$prev$$ refers to the old $$dp[j-1]$$.

```python
class Solution(object):
    def maximalSquare(self, matrix):
        """
        :type matrix: List[List[str]]
        :rtype: int
        """
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return 0

        m, n = len(matrix[0]), len(matrix)

        dp = [0] * m
        maxlen = 0
        prev = 0
        for i in range(n):
            for j in range(m):
                if matrix[i][j] == "1":
                    new_prev = dp[j]
                    dp[j] = min(dp[j], dp[j - 1], prev) + 1
                    if j == m - 1:
                        prev = 0
                    else:
                        prev = new_prev
                    if dp[j] > maxlen:
                        maxlen = dp[j]
                else:
                    dp[j] = 0

        return maxlen * maxlen
```

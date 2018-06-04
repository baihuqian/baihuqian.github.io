---
layout: "post"
title: "Leetcode 70: Climbing Stairs"
date: "2018-06-01 00:06"
tags:
  - Leetcode
---

# Question
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.

Example 1:
```
Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
```

Example 2:
```
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

```

# Solution
To climb `n` steps, one can climb first `n-1` steps and climb the last 1 step, or climb the first `n-2` step and climb the last 2 steps. Thus, the answer for `n` is the sum of those for `n-1` and `n-2`.

```python
class Solution:
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n == 1:
            return 1
        if n == 2:
            return 2

        dp = [1, 2]
        for idx in range(2, n):
            dp.append(dp[idx - 1] + dp[idx - 2])

        return dp[-1]
```

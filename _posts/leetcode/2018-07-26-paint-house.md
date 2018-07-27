---
layout: "post"
title: "Leetcode 256: Paint House"
date: "2018-07-26 22:43"
tags:
  - Leetcode
  - Review
  - DP
---

# Question
There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a `n x 3` cost matrix. For example, `costs[0][0]` is the cost of painting house 0 with color red; `costs[1][2]` is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

Note:
All costs are positive integers.

Example:

```
Input: [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue.
             Minimum cost: 2 + 5 + 3 = 10.

```

# Solution
Use DP. The cost to paint the next house red is the minimum total cost to paint the previous house blue or green plus the cost to paint the current house red.

```python
class Solution(object):
    def minCost(self, costs):
        """
        :type costs: List[List[int]]
        :rtype: int
        """
        dpr = dpb = dpg = 0
        for r, b, g in costs:
            dpr, dpb, dpg = min(dpb, dpg) + r, min(dpr, dpg) + b, min(dpr, dpb) + g
        return min(dpr, dpb, dpg)
```

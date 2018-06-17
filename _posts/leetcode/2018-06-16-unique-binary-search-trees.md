---
layout: "post"
title: "Leetcode 96: Unique Binary Search Trees"
date: "2018-06-16 15:19"
tags:
  - Leetcode
---

# Question
Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?

Example:

```
Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

# Solution
```python
class Solution:
    def numTrees(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n == 1:
            return 1
        dp = [0 for _ in range(n + 1)]
        dp[1] = 1

        for i in range(2, n + 1):
            s = 0
            for left in range(0, i):
                right = i - 1 - left
                if left == 0 or right == 0:
                    s += dp[left] + dp[right]
                else:
                    s += dp[left] * dp[right]
            dp[i] = s

        return dp[-1]
```

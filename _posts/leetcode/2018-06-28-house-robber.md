---
layout: post
title: 'Leetcode 198: House Robber'
date: '2018-06-28 23:06'
tags:
  - Leetcode
  - DP
---

# Question
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

Example 1:
```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

Example 2:
```
Input: [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

# Solution
It can be overwhelming to think about all the possible cases. However, it can be reduced to dynamic programming. Assuming `f(k)` is the maximum amount that can be robbed from first `k` houses, obviously we have `f(1) = A1`, and `f(2) = max(A1, A2)`. For house k, there are two options:

1. Rob house k. It will be Ak + f(k - 2).
2. Do not rob house k. It will be f(k - 1).

If we assume `f(-1) = 0` and `f(0) = 0`, then `f(k) = max(f(k - 2) + Ak, f(k - 1))`. We don't need an array to store `f` as we only need the previous two values.

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        prevMax = 0
        currMax = 0
        for n in nums:
            temp = currMax
            currMax = max(prevMax + n, temp)
            prevMax = temp

        return currMax
```

---
layout: "post"
title: "Leetcode 213: House Robber II"
date: "2018-06-28 23:19"
tags:
  - Leetcode
  - DP
---

# Question
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

Example 1:
```
Input: [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2),
             because they are adjacent houses.
```

Example 2:
```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

# Solution
Similar to House Robber problem, the only difference is that there are two choices for the first house. If robbing first house, then the last house cannot be robbed; if not robbing the first house, then the last house can be robbed. Then the problem can be reduced to two "linear" rob problems.

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            return 0
        elif len(nums) == 1:
            return nums[0]
        else:
            return max(self.linear_rob(nums[:-1]), self.linear_rob(nums[1:]))

    def linear_rob(self, nums):
        prevMax = 0
        currMax = 0
        for n in nums:
            temp = currMax
            currMax = max(prevMax + n, temp)
            prevMax = temp
        return currMax
```

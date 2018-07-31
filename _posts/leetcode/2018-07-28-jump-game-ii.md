---
layout: post
title: 'Leetcode 45: Jump Game II'
date: '2018-07-28 20:42'
tags:
  - Leetcode
  - Hard
  - Review
published: true
---

# Question
Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

Example:

```
Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

Note:

You can assume that you can always reach the last index.

# $$O(n)$$ Greedy Solution
This solution maintains three variables:
1. the maximum index reachable from `0` to `i`, denoted as `max_reachable`;
2. the furthest we can reach within the current number of jumps, denoted as `furthest`. In other words, you can go `0` to `j` where `j <= furthest` in less than or equal to `jumps` steps.
3. the number of jumps made so far, denoted as `jumps`.

When `i == furthest`, we have to make another jump, and the furthest within that jump is the current `max_reachable`. If `max_reachable` is beyond the last index, then we know we can reach the last index within the next jump.

The invariant for the loop is that given the number of jumps, our jumps can reach farther than, or as far as any other jumps (`furthest` holds the jumping distance). So If there is an optimal solution and the minimal jumps needed is `jumps`, then we could argue that given jumps, our jumps can reach as far as the optimal solution, thus our solution is also optimal.

```python
class Solution(object):
    def jump(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) <= 1:
            return 0

        jumps = 0
        furthest = 0
        max_reachable = 0

        for i in range(len(nums) - 1):
            max_reachable = max(max_reachable, i + nums[i])
            if i == furthest:
                jumps += 1
                if max_reachable >= len(nums) - 1:
                    return jumps
                else:
                    furthest = max_reachable

        return jumps
```

# $$O(n^2)$$ DP Solution

```python
class Solution(object):
    def jump(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) <= 1:
            return 0

        dp = [0 for _ in range(len(nums))]
        for i in range(len(dp) - 2, -1, -1):
            if i + nums[i] >= len(nums):
                dp[i] = 1
            elif nums[i] == 0:
                dp[i] = float('inf')
            else:
                dp[i] = min(dp[i+1: i + nums[i] + 1]) + 1

        return dp[0]
```

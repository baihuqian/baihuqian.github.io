---
layout: "post"
title: "Leetcode 53: Maximum Subarray"
date: "2018-05-31 23:26"
tags:
  - Leetcode
  - DP
---

# Question
Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Example:
```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

Follow up:

If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

# Solution
Apparently, this is a optimization problem, which can be usually solved by DP. So when it comes to DP, the first thing for us to figure out is the format of the sub problem. The format of the sub problem can be helpful when we are trying to come up with the recursive relation.

In this problem, the sub problem should be the maximum subarray ending at position `i`. It is either the maximum subarray ending at position `i-1` when the sum of subarray is positive, or `nums[i]`.

```python
class Solution:
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            return 0

        dp = [0] * len(nums)
        dp[0] = nums[0]
        max_sum = dp[0]

        for idx in range(1, len(nums)):
            if dp[idx - 1] > 0:
                dp[idx] = dp[idx - 1] + nums[idx]
            else:
                dp[idx] = nums[idx]
            if dp[idx] > max_sum:
                max_sum = dp[idx]

        return max_sum
```

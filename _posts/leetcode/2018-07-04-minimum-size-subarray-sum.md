---
layout: post
title: 'Leetcode 209: Minimum Size Subarray Sum'
date: '2018-07-04 21:33'
tags:
  - Leetcode
  - SlidingWindow
---

# Question
Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.

Example:

```
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
```

Follow up:

If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n).

# Solution
```python
class Solution(object):
    def minSubArrayLen(self, s, nums):
        """
        :type s: int
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            return 0

        left, right = 0, 0
        minLen = len(nums) + 1
        sum = 0
        while right < len(nums):
            sum += nums[right]
            if sum >= s:
                minLen = min(minLen, right - left + 1)
            while sum - nums[left] >= s and left < right:
                sum = sum - nums[left]
                left += 1
                minLen = min(minLen, right - left + 1)
            right += 1

        return 0 if minLen == len(nums) + 1 else minLen
```

---
layout: "post"
title: "Leetcode 34: Search for a Range"
date: "2018-05-22 22:54"
tags:
  - Leetcode
---

# Question
Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

Example 1:
```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

Example 2:
```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

# Solution
Use binary search to find the leftmost and rightmost index that equal to target.

```python
class Solution:
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if len(nums) == 0:
            return [-1, -1]
        if len(nums) == 1:
            if nums[0] == target:
                return [0, 0]
            else:
                return [-1, -1]

        # left
        l, r = 0, len(nums) - 1
        while r - l > 1:
            m = (l + r) // 2
            if nums[m] >= target:
                r = m
            else:
                l = m

        if nums[l] == target:
            left = l
        elif nums[l+1] == target:
            left = l + 1
        else:
            return [-1, -1]

        # right
        # left
        l, r = 0, len(nums) - 1
        while r - l > 1:
            m = (l + r) // 2
            if nums[m] <= target:
                l = m
            else:
                r = m

        if nums[r] == target:
            right = r
        elif nums[r-1] == target:
            right = r-1
        else:
            return [-1, -1]

        return [left, right]
```

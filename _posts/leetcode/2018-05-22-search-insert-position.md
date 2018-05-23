---
layout: "post"
title: "Leetcode 35: Search Insert Position"
date: "2018-05-22 22:39"
tags:
  - Leetcode
---

# Question

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Example 1:
```
Input: [1,3,5,6], 5
Output: 2
```

Example 2:
```
Input: [1,3,5,6], 2
Output: 1
```

Example 3:
```
Input: [1,3,5,6], 7
Output: 4
```

Example 4:
```
Input: [1,3,5,6], 0
Output: 0
```

# Solution
```python
class Solution:
    def searchInsert(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if len(nums) == 0:
            return 0

        length = len(nums)
        if target <= nums[0]:
            return 0
        elif target > nums[-1]:
            return length

        l, r = 0, length
        while r - l > 1:
            m = (r + l) // 2
            if nums[m] == target:
                return m
            elif target > nums[m]:
                l = m
            else:
                r = m
        return r
```

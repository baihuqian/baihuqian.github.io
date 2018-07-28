---
layout: post
title: 'Leetcode 81: Search in Rotated Sorted Array II'
date: '2018-06-14 22:25'
tags:
  - Leetcode
  - BinarySearch
  - Hard
  - Review
---

# Question
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,0,1,2,2,5,6] might become [2,5,6,0,0,1,2]).

You are given a target value to search. If found in the array return true, otherwise return false.

Example 1:
```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

Example 2:
```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

Follow up:

* This is a follow up problem to Search in Rotated Sorted Array, where nums may contain duplicates.
* Would this affect the run-time complexity? How and why?

# Solution
```python
class Solution:
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: bool
        """
        if len(nums) == 0:
            return False

        left, right = 0, len(nums) - 1

        while left < right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return True
            # right is ordered
            if nums[mid] < nums[right]:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
            # left is ordered
            elif nums[mid] > nums[right]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                right -= 1

        return nums[left] == target
```

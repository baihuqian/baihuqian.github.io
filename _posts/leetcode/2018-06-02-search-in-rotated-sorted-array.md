---
layout: post
title: "Leetcode 33: Search in Rotated Sorted Array"
date: '2018-06-02 23:34'
tags:
  - Leetcode
  - BinarySearch
---

# Question
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log n).

Example 1:
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

Example 2:
```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

# Solution
Use binary search.

```python
class Solution:
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if len(nums) == 0:
            return -1

        left, right = 0, len(nums) - 1

        if nums[left] == target:
            return left
        elif nums[right] == target:
            return right

        while True:      
            if right - left <= 1:
                return -1

            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            if nums[left] < target < nums[mid]:
                right = mid
            elif nums[mid] < target < nums[right]:
                left = mid
            elif nums[mid] > nums[left]:
                left = mid
            elif nums[right] > nums[mid]:
                right = mid
            else:
                return -1
```

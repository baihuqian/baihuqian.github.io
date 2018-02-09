---
layout: "post"
title: "Leetcode 283: Move Zeroes"
date: "2016-02-06 00:16"
tags: Leetcode
---

# Question
Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

For example, given `nums = [0, 1, 0, 3, 12]`, after calling your function, `nums` should be `[1, 3, 12, 0, 0]`.

Note:
You must do this in-place without making a copy of the array.
Minimize the total number of operations.

# Solution

```python
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        size = len(nums)
        index = 0
        while index < size:
            if nums[index] == 0:
                del nums[index]
                nums.append(0)
                size -= 1
            else:
                index += 1
```

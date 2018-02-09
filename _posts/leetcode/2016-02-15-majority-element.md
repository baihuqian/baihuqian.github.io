---
layout: "post"
title: "Leetcode 169: Majority Element"
date: "2016-02-15 00:23"
tags: Leetcode
---

# Question
Given an array of size n, find the majority element. The majority element is the element that appears more than `⌊ n/2 ⌋` times.

You may assume that the array is non-empty and the majority element always exist in the array.

# Solution
```python
class Solution(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 1:
            return nums[0]
        else:
            return self.find_majority(nums)

    def find_majority(self, nums):
        maj_index = 0
        count = 1
        for i in range(1, len(nums)):
            if nums[maj_index] == nums[i]:
                count += 1
            else:
                count -= 1
            if count == 0:
                count = 1
                maj_index = i
        return nums[maj_index]

```

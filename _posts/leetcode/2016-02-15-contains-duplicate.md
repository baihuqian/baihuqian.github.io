---
layout: "post"
title: "Leetcode 217: Contains Duplicate"
date: "2016-02-15 00:12"
tags: Leetcode
---

# Question
Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

# Solution

```python
class Solution(object):
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        return len(set(nums)) != len(nums)
```

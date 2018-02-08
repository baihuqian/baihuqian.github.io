---
layout: "post"
title: "Leetcode 136: Single Number"
date: "2016-11-07 18:46"
tags: Leetcode
---
# Question

Given an array of integers, every element appears twice except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

# Solution

A simple solution is to count the number of occurrences for each number, and find the one with only one occurrence.

```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 1:
            return nums[0]
        num_count = dict()
        for num in nums:
            if num not in num_count:
                num_count[num] = 1
            else:
                num_count[num] = 2

        for k in num_count:
            if num_count[k] == 1:
                return k


```

A more memory-efficient and faster way is to use XOR. Because the XOR'ing the same number twice will be no effect, thus the value after XOR'ing all numbers together will be the one with only one occurrence.

```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        res = 0
        for n in nums:
            res ^= n

        return res

```

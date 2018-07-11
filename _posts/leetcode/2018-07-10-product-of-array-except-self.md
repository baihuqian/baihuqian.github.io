---
layout: "post"
title: "Leetcode 238: Product of Array Except Self"
date: "2018-07-10 22:05"
tags:
  - Leetcode
  - Review
---

# Question
Given an array `nums` of n integers where `n > 1`,  return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

Example:
```
Input:  [1,2,3,4]
Output: [24,12,8,6]
```

Note: Please solve it without division and in O(n).

Follow up:

Could you solve it with constant space complexity? (The output array does not count as extra space for the purpose of space complexity analysis.)

# Solution
We can do this in two parts. The first part is to get the multiplication for all numbers left to position `i`. This can be done left to right by multiplying the value at position `i-1` with `nums[i-1]`. The second part is to get the multiplication for all numbers right to position `i`. This can be done right to left.

```python
class Solution(object):
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        res = list()
        res.append(1)

        for i in range(1, len(nums)):
            res.append(res[-1] * nums[i - 1])

        right = 1
        for i in range(len(nums) - 1, -1, -1):
            res[i] *= right
            right *= nums[i]

        return res
```

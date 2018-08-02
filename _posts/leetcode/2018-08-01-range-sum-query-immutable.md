---
layout: "post"
title: "Leetcode 303: Range Sum Query - Immutable"
date: "2018-08-01 22:42"
tags:
  - Leetcode
---

# Question
Given an integer array `nums`, find the sum of the elements between indices `i` and `j` (i ≤ j), inclusive.

Example:
```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

Note:

1. You may assume that the array does not change.
1. There are many calls to sumRange function.


# Solution
```python
class NumArray(object):

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        self.nums = nums
        self.dp = []

        for i in range(len(nums)):
            if i == 0:
                self.dp.append(nums[i])
            else:
                self.dp.append(self.dp[-1] + nums[i])

    def sumRange(self, i, j):
        """
        :type i: int
        :type j: int
        :rtype: int
        """
        if j < i:
            return 0
        elif j == i:
            return self.nums[i]
        else:
            return self.dp[j] - (self.dp[i - 1] if i >= 1 else 0)


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# param_1 = obj.sumRange(i,j)
```

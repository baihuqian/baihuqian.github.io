---
layout: "post"
title: "Leetcode 268: Missing Number"
date: "2018-07-28 17:35"
tags:
 - Leetcode
---

# Question
Given an array containing `n` distinct numbers taken from `0, 1, 2, ..., n`, find the one that is missing from the array.

Example 1:

```
Input: [3,0,1]
Output: 2
```

Example 2:

```
Input: [9,6,4,2,3,5,7,0,1]
Output: 8
```

Note:

Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

# Solution 1
The problem can be reduced to Single Number by XOR'ing all inputs together and XOR'ing with `0, 1, 2, ..., n`.
```python
class Solution(object):
    def missingNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        res = 0
        for i in range(1, len(nums) + 1):
            res ^= i

        for n in nums:
            res ^= n

        return res
```

# Solution 2
We know the sum of `0, 1, 2, ..., n` is $$n(n + 1) / 2$$. Thus, we subtract every number in `nums` from this sum and the result is the missing number.

```python
class Solution(object):
    def missingNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            res = 0
        if len(nums) == 1:
            res = 1
        else:
            res = len(nums) * (len(nums) + 1) // 2

        for n in nums:
            res -= n

        return res
```

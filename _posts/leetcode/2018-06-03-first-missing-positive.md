---
layout: post
title: 'Leetcode 41: First Missing Positive'
date: '2018-06-03 20:45'
tags:
  - Leetcode
  - Hard
  - Review
---

# Question
Given an unsorted integer array, find the smallest missing positive integer.

Example 1:
```
Input: [1,2,0]
Output: 3
```

Example 2:
```
Input: [3,4,-1,1]
Output: 2
```

Example 3:
```
Input: [7,8,9,11,12]
Output: 1
```

Note:

Your algorithm should run in O(n) time and uses constant extra space.

# Solution
Place number `n` in the `n`th place in the array (indexed at `n-1`). Find the first mismatch number.

```python
class Solution:
    def firstMissingPositive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            return 1

        n = len(nums)
        for i in range(0, n):
            while nums[i] > 0 and nums[i] <= n and nums[nums[i]-1] != nums[i]:
                nums[nums[i]-1], nums[i] = nums[i], nums[nums[i]-1]

        for i in range(0, n):
            if nums[i] != i + 1:
                return i + 1

        return n + 1

```

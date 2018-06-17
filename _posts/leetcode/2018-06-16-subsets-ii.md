---
layout: post
title: 'Leetcode 90: Subsets II'
date: '2018-06-16 13:40'
tags:
  - Leetcode
---

# Question
Given a collection of integers that might contain duplicates, nums, return all possible subsets (the power set).

Note: The solution set must not contain duplicate subsets.

Example:

```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

# Solution
```python
from collections import Counter
class Solution:
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """

        res = [[]]
        if len(nums) == 0:
            return res

        nums_count = dict(Counter(nums))
        nums = list(nums_count.keys())
        nums.sort()
        self.backtracking(nums, nums_count, 0, [], res)
        return res

    def backtracking(self, nums, nums_count, last_index, partial, res):
        for i in range(last_index, len(nums)):
            num = nums[i]
            if nums_count[num] > 0:
                nums_count[num] -= 1
                new_partial = partial + [num]
                res.append(new_partial)
                self.backtracking(nums, nums_count, i, new_partial, res)
                nums_count[num] += 1
                
```

---
layout: "post"
title: "Leetcode 16: 3Sum Closest"
date: "2018-05-21 22:23"
tags:
  - Leetcode
---

# Question
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

Example:
```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

# Solution
Sort the input array. For each number from small to large, so the following:
1. if the sum of smallest three numbers from this number (which are i, i+1 and i+2) is larger than the target, then the closest sum to target with numbers from i is this.
2. if the sum of the largest three numbers from ith number (which are i, len(nums) - 1 and len(nums) - 2) is smaller than the target, then the closest sum to target with numbers from i is this.
3. else, scan from i to len(nums)-1 using 2Sum method, but store the closest sum to target.

It is an $$O(n^2)$$ solution with $$O(1)$$ space.

```python
class Solution:
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if len(nums) == 3:
            return sum(nums)

        nums.sort()
        closest = sum(nums[:3])

        length = len(nums)
        for idx, n in enumerate(nums[:-2]):
            l, r = idx + 1, length - 1
            smin = nums[idx] + nums[idx + 1] + nums[idx + 2]
            smax = nums[idx] + nums[r] + nums[r - 1]
            if smin > target:
                if abs(smin - target) < abs(closest - target):
                    closest = smin
            elif smax < target:
                if abs(smax - target) < abs(closest - target):
                    closest = smax
            else:
                while l < r:
                    s = nums[idx] + nums[l] + nums[r]
                    if abs(s - target) < abs(closest - target):
                        closest = s
                    if s < target:
                        l += 1
                    elif s > target:
                        r -= 1
                    else:
                        return target

        return closest
```            

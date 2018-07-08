---
layout: post
title: 'Leetcode 219: Contains Duplicate II'
date: '2018-07-06 21:58'
tags:
  - Leetcode
  - SlidingWindow
---

# Question
Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the absolute difference between i and j is at most k.

Example 1:
```
Input: nums = [1,2,3,1], k = 3
Output: true
```

Example 2:

```
Input: nums = [1,0,1,1], k = 1
Output: true
```

Example 3:

```
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```

# Solution
```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        """
        if k == 0 or len(nums) == 0:
            return False
        if len(nums) <= k:
            return len(set(nums)) != len(nums)

        d = dict()
        for n in nums[:k+1]:
            if n in d:
                return True
            else:
                d[n] = 1
        for i in range(1, len(nums) - k):
            n = nums[i-1]
            if d[n] == 1:
                del d[n]
            else:
                d[n] -= 1
            n = nums[i+k]
            if n in d:
                return True
            else:
                d[n] = 1

        return False
        
```

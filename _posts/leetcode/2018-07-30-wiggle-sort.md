---
layout: "post"
title: "Leetcode 280: Wiggle Sort"
date: "2018-07-30 20:41"
tags:
  - Leetcode
---

# Question
Given an unsorted array `nums`, reorder it in-place such that `nums[0] <= nums[1] >= nums[2] <= nums[3]....`

Example:

```
Input: nums = [3,5,2,1,6,4]
Output: One possible answer is [3,5,1,6,2,4]
```

# Solution 1
Sort the array first, then rearrange it into wiggeled order.
```python
class Solution(object):
    def wiggleSort(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        nums.sort()

        i = 1
        while i + 1 < len(nums):
            nums[i], nums[i + 1] = nums[i + 1], nums[i]
            i += 2
```

# Solution 2
We can swap the number with the next one if the order is not right.
```python
class Solution(object):
    def wiggleSort(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        for i in range(len(nums) - 1):
            if (i % 2 == 0) == (nums[i] > nums[i + 1]):
                nums[i], nums[i + 1] = nums[i + 1], nums[i]
```

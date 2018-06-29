---
layout: "post"
title: "Leetcode 162: Find Peak Element"
date: "2018-06-27 21:32"
tags:
  - Leetcode
---

# Question
A peak element is an element that is greater than its neighbors.

Given an input array nums, where nums[i] ≠ nums[i+1], find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that nums[-1] = nums[n] = -∞.

Example 1:
```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

Example 2:
```
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5
Explanation: Your function can return either index number 1 where the peak element is 2,
             or index number 5 where the peak element is 6.
```

Note:

Your solution should be in logarithmic complexity.

# Solution
A simple scan through the array will yield an algorithm of $$O(n)$$ time complexity. To get logarithmic complexity, the intuitive way is to use binary search. However, how do we reduce the search space?

If the middle point is on an ascending slope (nums[i] < nums[i + 1]), then the peak should be on the right side. Otherwise, the middle point must be on a descending slope (nums[i] < nums[i - 1]), and the peak should be on the left side.

```python
class Solution(object):
    def findPeakElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) <= 1:
            return 0

        if nums[0] > nums[1]:
            return 0
        elif nums[-1] > nums[-2]:
            return len(nums) - 1
        else:
            left = 0
            right = len(nums) - 1

            while left < right:
                mid = (left + right) // 2
                if nums[mid] > nums[mid - 1] and nums[mid] > nums[mid + 1]:
                    return mid
                elif nums[mid] < nums[mid - 1]:
                    right = mid
                elif nums[mid] < nums[mid + 1]:
                    left = mid
```

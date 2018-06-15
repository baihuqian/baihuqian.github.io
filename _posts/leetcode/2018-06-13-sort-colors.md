---
layout: "post"
title: "Leetcode 75: Sort Colors"
date: "2018-06-13 22:45"
tags:
  - Leetcode
---

# Question
Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note: You are not suppose to use the library's sort function for this problem.

Example:
```
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

Follow up:

* A rather straight forward solution is a two-pass algorithm using counting sort. First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
* Could you come up with a one-pass algorithm using only constant space?

# Solution
```python
class Solution:
    def sortColors(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        if len(nums) <= 1:
            return

        left, right = 0, len(nums) - 1
        mid = -1

        while left < right:
            while nums[left] == 0 and left < right:
                left += 1

            while nums[right] == 2 and left < right:
                right -= 1

            if nums[left] == 2 and nums[right] == 0:
                nums[left] = 0
                nums[right] = 2
                left += 1
                right -= 1
            elif nums[left] == 2:
                nums[left] = nums[right]
                nums[right] = 2
                right -= 1
            elif nums[right] == 0:
                nums[right] = nums[left]
                nums[left] = 0
                left += 1
            else:
                mid = max(mid, left + 1)
                while mid < right:
                    if nums[mid] == 0:
                        nums[mid] = 1
                        nums[left] = 0
                        left += 1
                        break
                    elif nums[mid] == 2:
                        nums[mid] = 1
                        nums[right] = 2
                        right -= 1
                        break
                    else:
                        mid += 1
                if mid == right:
                    break
```

---
layout: post
title: 'Leetcode 42: Trapping Rain Water'
date: '2018-06-03 16:55'
tags:
  - Leetcode
  - Hard
  - Review
---

# Question
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

Example:

```
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

# Intuition
For each element in the array, we find the maximum level of water it can trap after the rain, which is equal to the minimum of maximum height of bars on both the sides minus its own height. The maximum height of bars on the left and right of an element in the array can be calculated using Dynamic Programming. Moreover, if there is a longer bar on the other side (say right), the water trapped on the left is determined only by the maximum height of bars on the left side, which is smaller than that of the right. Thus, we can do the following:

Algorithm

* Initialize left pointer to 0 and right pointer to size-1
* While left < right, do:
  * If height[left] is smaller than height[right]:
    * If height[left]>=left_max, update left_max
    * Else add left_max−height[left] to ans
    * Add 1 to left.
  * Else:
    * If height[right]>=right_max, update right_max
    * Else add right_max−height[right] to ans
    * Subtract 1 from right.

# Solution

```python
class Solution:
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        if len(height) <= 2:
            return 0
        vol = 0
        left, right = 0, len(height) - 1
        max_left, max_right = height[left], height[right]
        while left < right:
            if height[left] < height[right]:
                if height[left] > max_left:
                    max_left = height[left]
                vol += max_left - height[left]
                left += 1
            else:
                if height[right] > max_right:
                    max_right = height[right]
                vol += max_right - height[right]
                right -= 1

        return vol
```

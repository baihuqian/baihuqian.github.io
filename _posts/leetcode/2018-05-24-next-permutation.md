---
layout: "post"
title: "Leetcode 31: Next Permutation"
date: "2018-05-24 23:03"
tags:
  - Leetcode
---

# Question
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

# Solution
From right to left, find the first value smaller than the one to its right. Swap it with the smallest number larger than it amongst all numbers to its right, and finally reverse the order for all numbers to its right.

[Full explanation](https://leetcode.com/problems/next-permutation/solution/).

```python
class Solution:
    def nextPermutation(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        if len(nums) < 2:
            return

        idx = len(nums) - 1
        while idx > 0:
            if nums[idx] > nums[idx - 1]:
                break
            idx -= 1

        pivot = idx - 1

        if pivot >= 0:
            while idx < len(nums) - 1:
                if nums[idx] >= nums[pivot] >= nums[idx + 1]:
                    break
                idx += 1

            swap = idx
            nums[pivot], nums[swap] = nums[swap], nums[pivot]

        l, r = pivot + 1, len(nums) - 1
        while l < r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1
            r -= 1

        return

```

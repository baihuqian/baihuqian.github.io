---
layout: "post"
title: "Leetcode 360: Sort Transformed Array"
date: "2018-08-12 20:18"
tags:
  - Leetcode
---

# Question
Given a sorted array of integers nums and integer values a, b and c. Apply a quadratic function of the form f(x) = ax2 + bx + c to each element x in the array.

The returned array must be in sorted order.

Expected time complexity: O(n)

Example 1:
```
Input: nums = [-4,-2,2,4], a = 1, b = 3, c = 5
Output: [3,9,15,33]
```

Example 2:

```
Input: nums = [-4,-2,2,4], a = -1, b = 3, c = 5
Output: [-23,-5,1,7]
```

# Solution
```python
class Solution:
    def sortTransformedArray(self, nums, a, b, c):
        """
        :type nums: List[int]
        :type a: int
        :type b: int
        :type c: int
        :rtype: List[int]
        """
        def calc(n):
            return a * n * n + b * n + c

        if len(nums) == 0:
            return []
        if a == 0:
            res = [calc(x) for x in nums]
            return res if b >= 0 else res[::-1]
        else:
            mid = -b / (2 * a)
            i = 0
            while i < len(nums) and nums[i] < mid:
                i += 1
            left, right = i - 1, i
            res = []
            nums = [calc(x) for x in nums]
            while left >= 0 and right < len(nums):
                if a > 0:
                    if nums[left] <= nums[right]:
                        res.append(nums[left])
                        left -= 1
                    else:
                        res.append(nums[right])
                        right += 1
                else:
                    if nums[left] >= nums[right]:
                        res.append(nums[left])
                        left -= 1
                    else:
                        res.append(nums[right])
                        right += 1

            while left >= 0:
                res.append(nums[left])
                left -= 1

            while right < len(nums):
                res.append(nums[right])
                right += 1

            return res if a > 0 else res[::-1]
```

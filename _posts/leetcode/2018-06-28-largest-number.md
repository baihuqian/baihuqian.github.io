---
layout: "post"
title: "Leetcode 179: Largest Number"
date: "2018-06-28 21:55"
---

# Question
Given a list of non negative integers, arrange them such that they form the largest number.

Example 1:

```
Input: [10,2]
Output: "210"
```

Example 2:

```
Input: [3,30,34,5,9]
Output: "9534330"
```
Note: The result may be very large, so you need to return a string instead of an integer.

# Solution
Use a custom sort function to sort the numbers. Python's `sort` function takes an keyword parameter `cmp` which takes a comparison function.

```python
class Solution(object):
    def largestNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: str
        """
        def numOrder(x, y):
            left = int(x + y)
            right = int(y + x)
            return left - right

        if len(nums) == 0:
            return ""
        numsStr = [str(n) for n in nums]
        numsStr.sort(reverse=True, cmp=numOrder)
        if numsStr[0] == '0':
            return "0"
        else:
            return "".join(numsStr)

```

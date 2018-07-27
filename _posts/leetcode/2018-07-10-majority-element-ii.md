---
layout: post
title: 'Leetcode 229: Majority Element II'
date: '2018-07-10 21:24'
tags:
  - Leetcode
  - Review
published: true
---

# Question
Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times.

Note: The algorithm should run in linear time and in O(1) space.

Example 1:
```
Input: [3,2,3]
Output: [3]
```

Example 2:
```
Input: [1,1,1,3,3,2,2,2]
Output: [1,2]
```

# Solution
Use modified Boyer-Moore algorithm. There are at most two majority elements, and the number of non-majority elements must be fewer than either of them. Thus, we can maintain two counters. If a number equals to one of the candidates, the corresponding counter +1 while the other does not change; if a number does not equal to either of them, both counters -1. If a counter is 0, replace the candidate with the next number. At the end, verify both of them occurred more than ⌊ n/3 ⌋ times, because some times there is only one majority element.

The overall time complexity is $$O(n)$$ (one pass for finding the majority candidates, and one pass for verification).

```python
class Solution(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if len(nums) == 0:
            return []
        elif len(nums) == 1:
            return nums

        candidate1, candidate2, count1, count2 = 0, 1, 0, 0

        for n in nums:
            if n == candidate1:
                count1 += 1
            elif n == candidate2:
                count2 += 1
            elif count1 == 0:
                count1 = 1
                candidate1 = n
            elif count2 == 0:
                count2 = 1
                candidate2 = n
            else:
                count1 -= 1
                count2 -= 1

        return [n for n in (candidate1, candidate2) if nums.count(n) > len(nums) // 3]

```

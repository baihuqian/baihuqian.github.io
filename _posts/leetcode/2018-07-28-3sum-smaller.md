---
layout: "post"
title: "Leetcode 259: 3Sum Smaller"
date: "2018-07-28 16:15"
tags:
 - Leetcode
 - SlidingWindow
 - Review
---

# Question
Given an array of n integers nums and a target, find the number of index triplets `i, j, k` with `0 <= i < j < k < n` that satisfy the condition `nums[i] + nums[j] + nums[k] < target`.

Example:

```
Input: nums = [-2,0,1,3], and target = 2
Output: 2
Explanation: Because there are two triplets which sums are less than 2:
             [-2,0,1]
             [-2,0,3]
```

Follow up: Could you solve it in $$O(n^2)$$ runtime?

# Solution
Key observation: the answer does not change if numbers are swapped. Thus we can sort the numbers first and use a sliding window method.

We count the number of such combinations for each $$i$$, so that we can reduce the problem to 2Sum smaller. For 2Sum Smaller, we initialize two indices, `left` and `right`, pointing to the first and last element respectively. If `nums[left] + nums[right] >= target - nums[i]`, then there are no combinations with `i` and `k = right` (because `j = left` is the smallest), and we need to reduce `right`. Else, there are `right - left` pair of `(j, k)` when `j = left`, and we can add them to the number of combinations and increment `left`.

```python
class Solution(object):
    def threeSumSmaller(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if len(nums) < 3:
            return 0

        sums = 0
        nums.sort()
        for i in range(len(nums) - 2):
            sums += self.__twoSumSmaller(nums[i+1:], target - nums[i])

        return sums

    def __twoSumSmaller(self, nums, target):
        sums = 0
        left, right = 0, len(nums) - 1
        while left < right:
            if nums[left] + nums[right] < target:
                sums += right - left
                left += 1
            else:
                right -= 1

        return sums
```

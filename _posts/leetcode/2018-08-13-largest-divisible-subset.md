---
layout: "post"
title: "Leetcode 368: Largest Divisible Subset"
date: "2018-08-13 22:13"
tags:
  - Leetcode
  - Review
  - DP
---

# Question
Given a set of distinct positive integers, find the largest subset such that every pair (Si, Sj) of elements in this subset satisfies: Si % Sj = 0 or Sj % Si = 0.

If there are multiple solutions, return any subset is fine.

Example 1:
```
nums: [1,2,3]

Result: [1,2] (of course, [1,3] will also be ok)
```

Example 2:

```
nums: [1,2,4,8]

Result: [1,2,4,8]
```

# Solution
The key concept here is:

Given a set of integers that satisfies the property that each pair of integers inside the set are mutually divisible, for a new integer S, S can be placed into the set as long as it can divide the smallest number of the set or is divisible by the largest number of the set.

Thus, if we sort the list, and for each index `i` we track the size of the largest set so far that contains `nums[i]` in an array `count`, then for every index `j`, we need to find the largest `count[i]` such that `nums[j] % nums[i] == 0` and `0 <= i < j`.

In order to build the largest set, we also need to track `i` with the largest set and `nums[j] % nums[i] == 0` for each index `j`. Then we can backtrace from the `j` with largest subset backwards.

The time complexity is $$O(n^2)$$ and space complexity is $$O(n)$$.

```python
class Solution:
    def largestDivisibleSubset(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if len(nums) == 0:
            return []

        nums.sort()

        count = [0] * len(nums)
        parent = [-1] * len(nums)
        maxCount = 0
        idx = -1

        for i in range(len(nums)):
            for j in range(i - 1, -1, -1):
                if nums[i] % nums[j] == 0 and count[i] < count[j] + 1:
                    count[i] = count[j] + 1
                    parent[i] = j
                    if count[i] > maxCount:
                        maxCount = count[i]
                        idx = i

        if maxCount == 0:
            return [nums[0]]
        res = []
        while idx != -1:
            res.append(nums[idx])
            idx = parent[idx]

        return res
```

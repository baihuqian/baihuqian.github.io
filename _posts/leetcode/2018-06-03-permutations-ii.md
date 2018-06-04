---
layout: post
title: 'Leetcode 43: Permutations II'
date: '2018-06-03 21:56'
tags:
  - Leetcode
  - Backtracking
---

# Question
Given a collection of numbers that might contain duplicates, return all possible unique permutations.

Example:

```
Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

# Solution

```python
from collections import Counter

class Solution:
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if len(nums) == 0:
            return []

        result = []
        partial = []
        remaining = dict(Counter(nums))

        def backtrack(result, partial, remaining):
            if len(remaining) == 0:
                result.append(list(partial))
            else:
                key = set(remaining.keys())
                for n in key:
                    partial.append(n)
                    if remaining[n] == 1:
                        del remaining[n]
                    else:
                        remaining[n] -= 1      

                    backtrack(result, partial, remaining)

                    partial.pop()
                    if not n in remaining:
                        remaining[n] = 1
                    else:
                        remaining[n] += 1

        backtrack(result, partial, remaining)
        return result
```

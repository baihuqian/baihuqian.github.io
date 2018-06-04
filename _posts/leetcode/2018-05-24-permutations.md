---
layout: post
title: "Leetcode 46: Permutations"
date: '2018-05-24 23:23'
tags:
  - Leetcode
  - Backtracking
---

# Question
Given a collection of distinct integers, return all possible permutations.

Example:
```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

# Solution
Be careful with pass-by-reference. Copies of lists must be made to create separate results.

```python
class Solution:
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        def p(ns):
            if len(ns) == 1:
                return [list(ns)]
            res = []
            for n in ns:
                new_ns = set(ns)
                new_ns.remove(n)
                for m in p(new_ns):
                    m.append(n)
                    res.append(m)

            return res

        return p(set(nums))

```

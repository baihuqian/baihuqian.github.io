---
layout: post
title: "Leetcode 40: Combination Sum II"
date: '2018-06-02 23:54'
tags:
  - Leetcode
  - Backtracking
---

# Question
Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

Each number in candidates may only be used once in the combination.

Note:

All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.

Example 1:
```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

Example 2:
```
Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
```

# Solution
Use backtracking. Because candidates can contain duplicate numbers, use a set to dedupe results.

```python
class Solution:
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        solutions = set()
        progress = list()
        candidates.sort()

        def backtracking(candidates, target, solutions, progress, current_sum, last_index):
            if current_sum == target:
                solutions.add(tuple(progress))
                progress = list()
            else:
                diff = target - current_sum
                for idx in range(last_index + 1, len(candidates)):
                    if candidates[idx] <= diff:
                        progress.append(candidates[idx])
                        backtracking(candidates, target, solutions, progress, current_sum + candidates[idx], idx)
                        if len(progress) > 0:
                            progress.pop()
                    else:
                        break

        backtracking(candidates, target, solutions, progress, 0, -1)
        return list(solutions)

```

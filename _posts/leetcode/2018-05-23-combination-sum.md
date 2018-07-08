---
layout: "post"
title: "Leetcode 39: Combination Sum"
date: "2018-05-23 22:57"
tags:
  - Leetcode
  - Backtracking
---

# Question
Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

The same repeated number may be chosen from candidates unlimited number of times.

Note:

* All numbers (including target) will be positive integers.
* The solution set must not contain duplicate combinations.

Example 1:

```
Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]
```

Example 2:
```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

# Solution
Use backtracking. A partial solution is 0 or more candidates with a sum smaller or equal to target. Each recursion adds a number larger or equal to the last iteration to eliminate duplication.

For more information on backtracking, see [this note](http://jeffe.cs.illinois.edu/teaching/algorithms/notes/03-backtracking.pdf).

Python objects are passed by reference. Thus, to make a copy of a list when a solution is found, use `list(a_list)` or `a_list[:]` to copy a new reference before adding to the result set. Or create a new list for every recursion with `progress + [c]`.

```python
class Solution:
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        solutions = list()
        progress = list()
        candidates.sort()
        size = len(candidates)

        def backtrack(last_index, remaining):
            if remaining < 0:
                return
            if remaining == 0:
                solutions.append(list(progress)) # copy progress
                return
            if remaining > 0:
                for idx in range(last_index, size):
                    c = candidates[idx]
                    if c <= remaining:
                        progress.append(c)
                        backtrack(idx, remaining - c)
                        progress.pop()
                return
        backtrack(0, target)
        return solutions

```

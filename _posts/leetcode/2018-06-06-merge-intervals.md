---
layout: post
title: 'Leetcode 56: Merge Intervals'
date: '2018-06-06 22:53'
tags:
  - Leetcode
---

# Question
Given a collection of intervals, merge all overlapping intervals.

Example 1:
```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

Example 2:
```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considerred overlapping.
```

# Intuition

If we sort the intervals by their start value, then each set of intervals that can be merged will appear as a contiguous "run" in the sorted list.

# Solution
```python
# Definition for an interval.
# class Interval:
#     def __init__(self, s=0, e=0):
#         self.start = s
#         self.end = e

class Solution:
    def merge(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: List[Interval]
        """
        if len(intervals) <= 1:
            return intervals

        intervals.sort(key=lambda x: x.start)
        res = list()
        current = intervals[0]
        for intv in intervals[1:]:
            if intv.start > current.end:
                res.append(current)
                current = intv
            else:
                current.end = intv.end if intv.end > current.end else current.end

        res.append(current)
        return res
```

---
layout: "post"
title: "Leetcode 57: Insert Interval"
date: "2018-07-29 14:19"
tags:
  - Leetcode
  - Hard
---

# Question
Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

Example 1:

```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

Example 2:

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

# Solution
```python
# Definition for an interval.
# class Interval(object):
#     def __init__(self, s=0, e=0):
#         self.start = s
#         self.end = e

class Solution(object):
    def insert(self, intervals, newInterval):
        """
        :type intervals: List[Interval]
        :type newInterval: Interval
        :rtype: List[Interval]
        """
        # Naive cases
        if len(intervals) == 0:
            return [newInterval]
        if newInterval.end < intervals[0].start:
            return [newInterval] + intervals
        if newInterval.start > intervals[-1].end:
            return intervals + [newInterval]

        res = list()
        idx = 0
        # Find intervals before the newInterval, non-overlapping
        while idx < len(intervals):
            if intervals[idx].end < newInterval.start:
                res.append(intervals[idx])
                idx += 1
            else:
                break

        # Find the start of the overlap
        start = min(intervals[idx].start, newInterval.start)
        while idx < len(intervals) - 1:
            if intervals[idx + 1].start <= newInterval.end:
                idx += 1
            else:
                break

        # When the newInterval fits into two intervals
        if newInterval.end < intervals[idx].start:
            end = newInterval.end
            res.append(Interval(start, end))
            res.extend(intervals[idx:])
            return res
        # When overlaps
        else:
            end = max(intervals[idx].end, newInterval.end)
            res.append(Interval(start, end))
            res.extend(intervals[idx+1:])
            return res

```

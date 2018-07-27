---
layout: "post"
title: "Leetcode 253: Meeting Rooms II"
date: "2018-07-26 20:45"
tags:
  - Leetcode
---

# Question
Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...] (si < ei)`, find the minimum number of conference rooms required.

Example 1:

```
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
```

Example 2:

```
Input: [[7,10],[2,4]]
Output: 1
```

# Solution
```python
# Definition for an interval.
# class Interval(object):
#     def __init__(self, s=0, e=0):
#         self.start = s
#         self.end = e

class Solution(object):
    def minMeetingRooms(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: int
        """
        if len(intervals) == 0:
            return 0

        intervals.sort(key=lambda i: i.start)
        rooms = list()
        for i in intervals:
            reuse = False
            for idx in range(len(rooms)):
                if rooms[idx] <= i.start:
                    rooms[idx] = i.end
                    reuse = True
                    break
            if not reuse:
                rooms.append(i.end)

        return len(rooms)
```

---
layout: "post"
title: "Leetcode 228: Summary Ranges"
date: "2018-07-09 22:26"
tags:
  - Leetcode
---

# Question
Given a sorted integer array without duplicates, return the summary of its ranges.

Example 1:
```
Input:  [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.
```

Example 2:
```
Input:  [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.
```

# Solution
```python
class Solution(object):
    def summaryRanges(self, nums):
        """
        :type nums: List[int]
        :rtype: List[str]
        """
        if len(nums) == 0:
            return list()

        res = list()
        start = nums[0]
        prev = nums[0]

        for n in nums[1:]:
            if n != prev + 1:
                if prev == start:
                    res.append(str(start))
                else:
                    res.append("{}->{}".format(start, prev))
                start = n

            prev = n

        if prev == start:
            res.append(str(start))
        else:
            res.append("{}->{}".format(start, prev))
        return res
```

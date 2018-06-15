---
layout: "post"
title: "Leetcode 88: Merge Sorted Array"
date: "2018-06-14 23:18"
tags:
  - Leetcode
---

# Question
Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:

* The number of elements initialized in nums1 and nums2 are m and n respectively.
* You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2.

Example:

```
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```

# Solution
```python
class Solution:
    def merge(self, nums1, m, nums2, n):
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: void Do not return anything, modify nums1 in-place instead.
        """
        if n == 0:
            return
        if m == 0:
            nums1[:n] = nums2

        x, y = m - 1, n - 1
        for i in range(m + n - 1, -1, -1):
            if x >= 0 and y >= 0:
                if nums1[x] >= nums2[y]:
                    nums1[i] = nums1[x]
                    x -= 1
                else:
                    nums1[i] = nums2[y]
                    y -= 1
            elif x >= 0:
                nums1[i] = nums1[x]
                x -= 1
            else:
                nums1[i] = nums2[y]
                y -= 1

```

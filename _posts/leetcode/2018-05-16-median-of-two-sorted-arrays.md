---
layout: post
title: 'Leetcode 4: Median of Two Sorted Arrays'
date: '2018-05-16 23:32'
tags:
  - Leetcode
  - Hard
  - Review
---

# Question
There are two sorted arrays `nums1` and `nums2` of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be `O(log (m+n))`.

Example 1:
```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```
Example 2:
```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

# Solution
Note, in the analysis the indexes start at 1, while in Python it starts at 0. Thus `//` produces the first index in the larger half.
```python
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        if len(nums1) >= len(nums2):
            b = nums1
            a = nums2
        else:
            b = nums2
            a = nums1

        m = len(a)
        n = len(b)
        if m == 0:
            if n == 0:
                raise ValueError
            elif n % 2 == 0:
                return (b[n // 2] + b[n // 2 - 1]) / 2
            else:
                return b[n // 2]

        imin = 0
        imax = m
        half_len = (m + n + 1) // 2
        while imin <= imax:
            i = (imin + imax) // 2
            j = half_len - i

            if i > 0 and j < n and a[i-1] > b[j]: # i too big
                imax = i - 1
            elif j > 0 and i < m and b[j-1] > a[i]: # i too small
                imin = i + 1
            else:
                if i == 0: max_of_left = b[j-1]
                elif j == 0: max_of_left = a[i-1]
                else: max_of_left = max(a[i-1], b[j-1])

                if (m + n) % 2 == 1:
                    return max_of_left

                if i == m: min_of_right = b[j]
                elif j == n: min_of_right = a[i]
                else: min_of_right = min(a[i], b[j])

                return (max_of_left + min_of_right) / 2.0
```

# Analysis
See original post [here](https://leetcode.com/articles/median-of-two-sorted-arrays/).

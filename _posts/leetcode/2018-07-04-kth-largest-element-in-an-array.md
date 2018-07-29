---
layout: "post"
title: "Leetcode 215: Kth Largest Element in an Array"
date: "2018-07-04 21:43"
tags:
  - Leetcode
  - QuickSelect
  - Review
---

# Question
Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Example 1:

```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

Example 2:

```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

Note:

You may assume k is always valid, 1 ≤ k ≤ array's length.

# Heap Solution
Use a min heap of size k to store the largest k items, and the head is the kth largest item. Time complexity is $$O(nlogk)$$.

```python
from heapq import *

class Solution(object):
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """

        heap = []
        for i in range(k):
            heappush(heap, nums[i])
        for i in range(k, len(nums)):
            if nums[i] > heap[0]:
                heapreplace(heap, nums[i])

        return heap[0]
```

# Quick Select Solution
Choose a pivot. Place numbers smaller than pivot to its left, and those larger than pivot to its right. If pivot is at position k, return. If position is smaller than k, do the same to its right. If pivot is larger than k, use the same algorithm to select (k - pos)th largest to the left subarray.

Time complexity is average $$O(n)$$ with worst case $$O(n^2)$$, depending on the choice of pivot.

```python
class Solution(object):
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        pos = self.partition(nums)
        if pos == len(nums) - k:
            return nums[pos]
        elif pos < len(nums) - k:
            return self.findKthLargest(nums[pos+1:], k)
        else:
            return self.findKthLargest(nums[:pos], pos + k - len(nums))

    def partition(self, nums):
        if len(nums) == 1:
            return 0
        pivot = self.medium_of_three(nums)
        left, right = 1, len(nums) - 1
        while left <= right:
            if nums[left] >= pivot >= nums[right]:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
                right -= 1
            else:
                if nums[left] <= pivot:
                    left += 1
                if nums[right] >= pivot:
                    right -= 1
        nums[left - 1], nums[0] = pivot, nums[left - 1]
        return left - 1

    def medium_of_three(self, nums):
        if len(nums) <= 2:
            return nums[0]
        else:
            medium = len(nums) // 2
            if nums[medium] <= nums[-1] <= nums[0] or nums[0] <= nums[-1] <= nums[medium]:
                nums[0], nums[-1] = nums[-1], nums[0]
            elif nums[0] <= nums[medium] <= nums[-1] or nums[-1] <= nums[medium] <= nums[0]:
                nums[0], nums[medium] = nums[medium], nums[0]

            return nums[0]
```

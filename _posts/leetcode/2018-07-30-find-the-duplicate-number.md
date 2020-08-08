---
layout: "post"
title: "Leetcode 287: Find the Duplicate Number"
date: "2018-07-30 22:41"
tags:
  - Leetcode
  - Review
---

# Question
Given an array `nums` containing `n + 1` integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

Example 1:

```
Input: [1,3,4,2,2]
Output: 2
```

Example 2:

```
Input: [3,1,3,4,2]
Output: 3
```

Note:

1. You must not modify the array (assume the array is read only).
2. You must use only constant, O(1) extra space.
3. Your runtime complexity should be less than O(n2).
4. There is only one duplicate number in the array, but it could be repeated more than once.

# Solution: Floyd's Tortoise and Hare (Cycle Detection)
If we interpret `nums` such that for each pair of index $$i$$ and value $$v_i$$, the "next" value $$v_j$$ is at index $$v_i$$, we can reduce this problem to cycle detection. See the solution to [Linked List Cycle II]({{ site.baseurl }}{% link _posts/leetcode/2018-06-24-linked-list-cycle-ii.md %}) for more details.

Because each number in `nums` is between $$1$$ and $$n$$, it will necessarily point to an index that exists. Therefore, the list can be traversed infinitely, which implies that there is a cycle. Additionally, because $$0$$ cannot appear as a value in `nums`, `nums[0]` cannot be part of the cycle. Therefore, traversing the array in this manner from `nums[0]` is equivalent to traversing a cyclic linked list.

```python
class Solution(object):
    def findDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        tortoise = nums[0]
        hare = nums[0]
        while True:
            tortoise = nums[tortoise]
            hare = nums[nums[hare]]
            if tortoise == hare:
                break

        # Find the "entrance" to the cycle.
        ptr1 = nums[0]
        ptr2 = tortoise
        while ptr1 != ptr2:
            ptr1 = nums[ptr1]
            ptr2 = nums[ptr2]

        return ptr1
```

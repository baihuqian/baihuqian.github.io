---
layout: post
title: 'Leetcode 61: Rotate List'
date: '2018-06-12 22:44'
tags:
  - Leetcode
---

# Question
Given a linked list, rotate the list to the right by k places, where k is non-negative.

Example 1:
```
Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
```

Example 2:
```
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL
```

# Solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def rotateRight(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        if k == 0:
            return head
        if head is None or head.next is None:
            return head

        end, prev, new_head, new_head_prev = head, None, head, None

        idx = 0
        while idx < k and end is not None:
            prev = end
            end = end.next
            idx += 1

        if idx < k:
            length = idx
            k = k % length
            if k == 0:
                return head

            end, prev = head, None
            idx = 0
            while idx < k and end is not None:
                prev = end
                end = end.next
                idx += 1

        if idx == k and end is None:
            return head

        while end is not None:
            prev = end
            end = end.next
            new_head_prev = new_head
            new_head = new_head.next

        prev.next = head
        if new_head_prev is not None:
            new_head_prev.next = None

        return new_head

```

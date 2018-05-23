---
layout: "post"
title: "Leetcode 19: Remove Nth Node From End of List"
date: "2018-05-20 22:25"
tags:
  - Leetcode
---

# Question
Given a linked list, remove the n-th node from the end of list and return its head.

Example:
```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

Note:

Given n will always be valid.

Follow up:

Could you do this in one pass?

# Solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        nth_prev = head
        tail = head
        for i in range(0, n):
            tail = tail.next

        if tail is None:
            head = head.next
            return head

        while tail.next is not None:
            nth_prev = nth_prev.next
            tail = tail.next

        nth_prev.next = nth_prev.next.next
        return head

```

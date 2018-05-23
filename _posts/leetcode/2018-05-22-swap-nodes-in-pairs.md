---
layout: "post"
title: "Leetcode 24: Swap Nodes in Pairs"
date: "2018-05-22 21:37"
tags:
  - Leetcode
---

# Question
Given a linked list, swap every two adjacent nodes and return its head.

Example:
```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

Note:

* Your algorithm should use only constant extra space.
* You may not modify the values in the list's nodes, only nodes itself may be changed.

# Solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def swapPairs(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if head is None or head.next is None:
            return head

        res = head.next
        p = head
        prev = None
        while p is not None and p.next is not None:
            one, two, three = p, p.next, p.next.next
            one.next = three
            two.next = one
            if prev is not None:
                prev.next = two
            prev = one
            p = three

        return res


```

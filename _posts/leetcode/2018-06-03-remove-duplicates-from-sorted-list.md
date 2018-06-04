---
layout: post
title: "Leetcode 83: Remove Duplicates from Sorted List"
date: '2018-06-03 22:08'
tags:
  - Leetcode
---

# Question
Given a sorted linked list, delete all duplicates such that each element appear only once.

Example 1:
```
Input: 1->1->2
Output: 1->2
```

Example 2:
```
Input: 1->1->2->3->3
Output: 1->2->3
```

# Solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def deleteDuplicates(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if head is None or head.next is None:
            return head

        prev = head
        curr = head.next
        value = prev.val

        while curr is not None:
            if curr.val != value:
                prev.next = curr
                prev = curr
                value = curr.val

            curr = curr.next

        prev.next = None
        return head

```

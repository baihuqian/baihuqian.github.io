---
layout: "post"
title: "Leetcode 21: Merge Two Sorted Lists"
date: "2018-05-20 16:01"
tags:
  - Leetcode
---

# Question
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

Example:
```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

# Solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        if l1 is None:
            return l2
        if l2 is None:
            return l1

        curr1, curr2 = l1, l2
        if l1.val <= l2.val:
            head = l1
            curr1 = l1.next
        else:
            head = l2
            curr2 = l2.next

        prev = head
        while curr1 is not None and curr2 is not None:
            if curr1.val <= curr2.val:
                prev.next = curr1
                curr1 = curr1.next
            else:
                prev.next = curr2
                curr2 = curr2.next

            prev = prev.next

        if curr1 is None:
            prev.next = curr2
        elif curr2 is None:
            prev.next = curr1

        return head

```

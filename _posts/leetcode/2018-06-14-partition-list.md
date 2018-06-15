---
layout: "post"
title: "Leetcode 86: Partition List"
date: "2018-06-14 23:10"
tags:
  - Leetcode
---

# Question
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

Example:

```
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5
```

# Solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def partition(self, head, x):
        """
        :type head: ListNode
        :type x: int
        :rtype: ListNode
        """
        if head is None or head.next is None:
            return head

        shead, scurr, lhead, lcurr = None, None, None, None

        curr = head
        while curr is not None:
            if curr.val < x:
                if shead is None:
                    shead = curr
                else:
                    scurr.next = curr
                scurr = curr
            else:
                if lhead is None:
                    lhead = curr
                else:
                    lcurr.next = curr
                lcurr = curr

            curr = curr.next

        if scurr is not None:
            scurr.next = lhead
        else:
            shead = lhead

        if lcurr is not None:
            lcurr.next = None

        return shead

```

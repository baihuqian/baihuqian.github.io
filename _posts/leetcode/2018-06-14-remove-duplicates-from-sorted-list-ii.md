---
layout: post
title: 'Leetcode 82: Remove Duplicates from Sorted List II'
date: '2018-06-14 22:58'
tags:
  - Leetcode
---

# Question
Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

Example 1:
```
Input: 1->2->3->3->4->4->5
Output: 1->2->5
```

Example 2:
```
Input: 1->1->1->2->3
Output: 2->3
```

# Solution
While iterating through the linked list, keep track of the previous distinct node and the first distinct node (aka new head). Don't forget to handle the last element and `next` of the previous distinct node.

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

        new_head = None
        prev = None
        curr = head
        curr_val = curr.val
        counter = 1

        while curr.next is not None:
            if curr.next.val == curr_val:
                counter += 1
            else:
                if counter == 1:
                    if new_head is None:
                        new_head = curr
                    if prev is not None:
                        prev.next = curr
                    prev = curr

                curr_val = curr.next.val
                counter = 1
            curr = curr.next

        # Handle the last element
        if prev is not None:
            if counter == 1:
                prev.next = curr
            else:
                prev.next = None
        if new_head is None and counter == 1:
            new_head = curr

        return new_head
```

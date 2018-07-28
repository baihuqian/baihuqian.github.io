---
layout: post
title: 'Leetcode 25: Reverse Nodes in k-Group'
date: '2018-05-22 21:55'
tags:
  - Leetcode
  - Hard
  - Review
---

# Question
Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

Example:

Given this linked list: 1->2->3->4->5

For k = 2, you should return: 2->1->4->3->5

For k = 3, you should return: 3->2->1->4->5

Note:

* Only constant extra memory is allowed.
* You may not alter the values in the list's nodes, only nodes itself may be changed.

# Solution
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseKGroup(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        if head is None:
            return head

        res = None
        prev = None
        p = head

        while True:
            one_node = p
            i = 1
            k_node = p
            while i < k and k_node is not None:
                k_node = k_node.next
                i += 1
            if k_node is None:
                break
            kone_node = k_node.next
            curr_node, next_node = one_node, one_node.next
            for i in range(1, k):
                node = next_node.next
                next_node.next = curr_node
                curr_node, next_node = next_node, node
            one_node.next = kone_node
            if prev is not None:
                prev.next = k_node
            if res is None:
                res = k_node
            prev = one_node
            p = kone_node

        return res if res is not None else head



```

---
layout: post
title: 'Leetcode 141: Linked List Cycle'
date: '2018-06-24 16:24'
tags:
  - Leetcode
---

# Question
Given a linked list, determine if it has a cycle in it.

Follow up:

Can you solve it without using extra space?

# Solution
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        if head is None or head.next is None:
            return False
        node = head.next

        while node is not None:
            if node == head:
                return True
            next_node = node.next
            node.next = head
            node = next_node

        return False
```

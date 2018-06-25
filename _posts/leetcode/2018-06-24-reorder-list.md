---
layout: "post"
title: "Leetcode 143: Reorder List"
date: "2018-06-24 17:05"
tags:
  - Leetcode
---

# Question
Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

You may not modify the values in the list's nodes, only nodes itself may be changed.

Example 1:

Given 1->2->3->4, reorder it to 1->4->2->3.
Example 2:

Given 1->2->3->4->5, reorder it to 1->5->2->4->3.

# Solution
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reorderList(self, head):
        """
        :type head: ListNode
        :rtype: void Do not return anything, modify head in-place instead.
        """
        if head is None or head.next is None:
            return

        stack = list()
        count = 0
        node = head
        while node is not None:
            stack.append(node)
            count += 1
            node = node.next

        node = head
        half = count // 2 if count % 2 == 1 else count // 2 - 1
        for i in range(half):
            next_node = node.next
            insertion = stack.pop()
            node.next = insertion
            insertion.next = next_node
            node = next_node

        if count % 2 == 1:
            node.next = None
        else:
            insertion = stack.pop()
            node.next = insertion
            insertion.next = None
```

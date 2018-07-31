---
layout: post
title: "Leetcode 142: Linked List Cycle II"
date: '2018-06-24 16:44'
tags:
  - Leetcode
  - Hard
  - Review
---

# Question
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Note: Do not modify the linked list.

Follow up:

Can you solve it without using extra space?

# Solution: Floyd's Tortoise and Hare
To find whether there is a circle, we can use two pointers, hare and tortoise, one advances twice as fast as the other. There exists a cycle when tortoise and hare reaches the same point, or hare reaches the end of the linked list.

Two pointers, one at head and the other at the meeting point of hare and tortoise, will meet at the start of the cycle if they travel at the same speed.

See [Leetcode solution](https://leetcode.com/problems/linked-list-cycle-ii/solution/) for detailed explanation.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if head is None or head.next is None:
            return None

        hare, tortoise = head, head
        while True:
            try:
                hare = hare.next.next
            except AttributeError:
                return None
            tortoise = tortoise.next
            if hare == tortoise:
                break

        node1, node2 = head, hare
        while node1 != node2:
            node1 = node1.next
            node2 = node2.next

        return node1
```

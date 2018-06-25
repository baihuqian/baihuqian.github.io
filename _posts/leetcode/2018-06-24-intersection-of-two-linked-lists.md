---
layout: "post"
title: "Leetcode 160: Intersection of Two Linked Lists"
date: "2018-06-24 21:50"
tags:
  - Leetcode
---

# Question
Write a program to find the node at which the intersection of two singly linked lists begins.


For example, the following two linked lists:
```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
```

begin to intersect at node c1.


Notes:

* If the two linked lists have no intersection at all, return null.
* The linked lists must retain their original structure after the function returns.
* You may assume there are no cycles anywhere in the entire linked structure.
* Your code should preferably run in O(n) time and use only O(1) memory.

# Solution
Get the length of both linked lists. If the last node does not equal, then there's no intersection. Then advance the longer list `lenA - lenB` steps from the head (so that during iteration, both lists reach the end at the same time) and then iterate through the linked list. Return the first match node.
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        if headA is None or headB is None:
            return None

        lengthA, lengthB = 1, 1
        nodeA = headA
        while nodeA.next is not None:
            nodeA = nodeA.next
            lengthA += 1
        nodeB = headB
        while nodeB.next is not None:
            nodeB = nodeB.next
            lengthB += 1

        if nodeA != nodeB:
            return None
        nodeA, nodeB = headA, headB
        step = 0
        if lengthA > lengthB:
            while step < lengthA - lengthB:
                nodeA = nodeA.next
                step += 1
        elif lengthA < lengthB:
            while step < lengthB - lengthA:
                nodeB = nodeB.next
                step += 1

        while nodeA != nodeB:
            nodeA = nodeA.next
            nodeB = nodeB.next

        return nodeA
```

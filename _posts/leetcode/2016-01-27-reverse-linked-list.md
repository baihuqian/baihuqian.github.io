---
layout: "post"
title: "Leetcode 206: Reverse Linked List"
date: "2016-01-27 21:20"
tags: Leetcode
---

# Question
Reverse a singly linked list.

# Solution
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if head == None or head.next == None:
            return head
        elif head.next.next == None:
            tmp = head.next
            tmp.next = head
            head.next = None
            return tmp
        else:
            a = head
            b = head.next
            c = head.next.next
            a.next = None
            while c != None:
                b.next = a
                a = b
                b = c
                c = b.next
            b.next = a
            return b

```

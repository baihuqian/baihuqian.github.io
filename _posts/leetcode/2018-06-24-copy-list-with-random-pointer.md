---
layout: "post"
title: "Leetcode 138: Copy List with Random Pointer"
date: "2018-06-24 15:00"
tags:
  - Leetcode
---

# Question
A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

# Solution
```python
# Definition for singly-linked list with a random pointer.
# class RandomListNode(object):
#     def __init__(self, x):
#         self.label = x
#         self.next = None
#         self.random = None

class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: RandomListNode
        :rtype: RandomListNode
        """
        if head is None:
            return None

        new_head = RandomListNode(head.label)
        self.copy(head, new_head, dict())
        return new_head

    def copy(self, node, new_node, node_cache):
        node_cache[node] = new_node
        if node.next is not None:
            if node.next in node_cache:
                new_node.next = node_cache[node.next]
            else:
                new_next = RandomListNode(node.next.label)
                new_node.next = new_next
                self.copy(node.next, new_next, node_cache)
        if node.random is not None:
            if node.random in node_cache:
                new_node.random = node_cache[node.random]
            else:
                new_random = RandomListNode(node.random.label)
                new_node.random = new_random
                self.copy(node.random, new_random, node_cache)
```

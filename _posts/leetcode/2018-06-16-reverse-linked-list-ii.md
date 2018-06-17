---
layout: post
title: 'Leetcode 92: Reverse Linked List II'
date: '2018-06-16 14:26'
tags:
  - Leetcode
---

# Question
Reverse a linked list from position m to n. Do it in one-pass.

Note: 1 ≤ m ≤ n ≤ length of list.

Example:

```
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
```

# Solution
```python
class Solution:
    def reverseBetween(self, head, m, n):
        """
        :type head: ListNode
        :type m: int
        :type n: int
        :rtype: ListNode
        """
        if m == n:
            return head

        m_prev, n_next = None, None
        m_node, n_node = None, None

        prev, node, next_node = None, head, None
        i = 1
        while node is not None:
            if i == m - 1:
                m_prev = node
            elif i == n + 1:
                n_next = node
            elif i == m:
                m_node = node
            elif i == n:
                n_node = node

            next_node = node.next
            if m < i <= n:
                node.next = prev

            prev = node
            node = next_node
            i += 1

        m_node.next = n_next
        if m_prev is not None:
            m_prev.next = n_node
            return head
        else:
            return n_node
                
```

---
layout: "post"
title: "Leetcode 234: Palindrome Linked List"
date: "2018-07-09 22:47"
tags:
  - Leetcode
---

# Question
Given a singly linked list, determine if it is a palindrome.

Example 1:
```
Input: 1->2
Output: false
```

Example 2:
```
Input: 1->2->2->1
Output: true
```

Follow up:
* Could you do it in O(n) time and O(1) space?

# Solution
```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def isPalindrome(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        if head is None:
            return True
        if head.next is None:
            return True

        # Find the middle point of the string
        slow, fast = head, head.next
        # True if number of elements is even
        even = True
        while fast.next is not None:
            fast = fast.next
            if fast.next is not None:
                fast = fast.next
                slow = slow.next
            else:
                even = False

        # Find the head of the 1st and 2nd half
        h1 = slow
        if even:
            h2 = slow.next
        else:
            h2 = slow.next.next

        # Reverse the linked list up to and including slow
        prev, curr, next = None, head, head.next
        while curr != slow:
            next = curr.next
            curr.next = prev
            prev = curr
            curr = next
        slow.next = prev

        # Compare both half of the string
        while h2 is not None:
            if h1.val != h2.val:
                return False
            else:
                h1 = h1.next
                h2 = h2.next

        return True
```

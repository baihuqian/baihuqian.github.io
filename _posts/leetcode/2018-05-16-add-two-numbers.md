---
layout: "post"
title: "Leetcode 2: Add Two Numbers"
date: "2018-05-16 21:44"
tags: Leetcode
---

# Question

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

# Solution
Iterate through the linked list and add digits by digits.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        carry = 0
        head = None
        prev = None
        while l1 is not None or l2 is not None:
            x = l1.val if l1 is not None else 0
            y = l2.val if l2 is not None else 0
            sum = x + y + carry
            if sum >= 10:
                carry = 1
                res = sum - 10
            else:
                carry = 0
                res = sum
            node = ListNode(res)
            if head is None:
                head = node
            if prev is not None:
                prev.next = node
            if l1 is not None:
              l1 = l1.next
            if l2 is not None:
              l2 = l2.next
            prev = node

        if carry == 1:
            node = ListNode(carry)
            prev.next = node

        return head
```

# Follow Up: Add Two Numbers II
You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Follow up:**
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

Example:
```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```

# Solution
Convert the linked list to int value, sum them, and convert the sum to string to get each digit by iterating through the string.

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        x = self.listToInt(l1)
        y = self.listToInt(l2)

        res = str(x + y)
        head = None
        prev = None

        for c in res:
            node = ListNode(int(c))
            if head is None:
                head = node
            if prev is not None:
                prev.next = node
            prev = node

        return head

    def listToInt(self, l):
        x = 0
        while True:
            x *= 10
            x += l.val
            if l.next == None:
                break
            else:
                l = l.next
        return x
```

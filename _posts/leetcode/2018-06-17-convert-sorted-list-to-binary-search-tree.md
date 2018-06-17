---
layout: "post"
title: "Leetcode 109: Convert Sorted List to Binary Search Tree"
date: "2018-06-17 14:28"
tags:
  - Leetcode
---

# Question
Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Example:

```

Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

# Solution
Use recursion to maintain tree order, but pass the next node in the list around so that the time complexity is $$O(N)$$.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def sortedListToBST(self, head):
        """
        :type head: ListNode
        :rtype: TreeNode
        """

        if head is None:
            return None

        iterator = head
        node = head
        size = 0
        while iterator is not None:
            iterator = iterator.next
            size += 1

        def builder(start, end, node):
            if end < start:
                return (None, node)
            elif end == start:
                root = TreeNode(node.val)
                return (root, node.next)
            else:
                mid = (start + end) // 2
                left, new_node = builder(start, mid - 1, node)
                root = TreeNode(new_node.val)
                new_node = new_node.next
                right, new_node = builder(mid + 1, end, new_node)
                root.left = left
                root.right = right
                return (root, new_node)

        return builder(0, size - 1, node)[0]

```

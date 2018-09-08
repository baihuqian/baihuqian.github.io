---
layout: "post"
title: "Leetcode 404: Sum of Left Leaves"
date: "2018-09-08 17:30"
tags:
  - Leetcode
---

# Question
Find the sum of all left leaves in a given binary tree.

Example:
```
    3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
```

# Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def sumOfLeftLeaves(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root is None:
            return 0
        else:
            return self._helper(root, False)

    def _helper(self, root, isLeftChild):
        if root.left is None and root.right is None:
            return root.val if isLeftChild else 0

        sum = 0
        if root.left is not None:
            sum += self._helper(root.left, True)
        if root.right is not None:
            sum += self._helper(root.right, False)

        return sum
```

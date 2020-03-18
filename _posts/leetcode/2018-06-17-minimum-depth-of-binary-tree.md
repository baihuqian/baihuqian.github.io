---
layout: post
title: 'Leetcode 111: Minimum Depth of Binary Tree'
date: '2018-06-17 14:39'
tags:
  - Leetcode
published: true
---

# Question
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

Example:

Given binary tree `[3,9,20,null,null,15,7]`,
```
    3
   / \
  9  20
    /  \
   15   7
```

return its minimum depth = 2.

# Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def minDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root is None:
            return 0

        if root.left is not None and root.right is not None:
            left = self.minDepth(root.left)
            right = self.minDepth(root.right)
            return min(left, right) + 1
        elif root.left is not None:
            return self.minDepth(root.left) + 1
        elif root.right is not None:
            return self.minDepth(root.right) + 1
        else:
            return 1
```

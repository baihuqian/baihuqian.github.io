---
layout: "post"
title: "Leetcode 114: Flatten Binary Tree to Linked List"
date: "2018-06-17 22:19"
tags:
  - Leetcode
---

# Question
Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:
```
    1
   / \
  2   5
 / \   \
3   4   6
```

The flattened tree should look like:
```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
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
    def flatten(self, root):
        """
        :type root: TreeNode
        :rtype: void Do not return anything, modify root in-place instead.
        """
        if root is None:
            return root

        stack = list()
        stack.append(root)
        prev = None
        while len(stack) > 0:
            node = stack.pop()
            if node.right is not None:
                stack.append(node.right)
                node.right = None
            if node.left is not None:
                stack.append(node.left)
                node.left = None
            if prev is not None:
                prev.right = node
            prev = node

```

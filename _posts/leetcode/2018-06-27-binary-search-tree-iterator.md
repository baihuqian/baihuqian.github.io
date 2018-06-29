---
layout: "post"
title: "Leetcode 173: Binary Search Tree Iterator"
date: "2018-06-27 22:27"
tags:
  - Leetcode
---

# Question
Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling next() will return the next smallest number in the BST.

Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

# Solution
```python
# Definition for a  binary tree node
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class BSTIterator(object):
    def __init__(self, root):
        """
        :type root: TreeNode
        """
        self.stack = []
        if root is not None:
            self.stack.append(root)
            self._push_left(root.left)


    def hasNext(self):
        """
        :rtype: bool
        """
        return len(self.stack) > 0


    def next(self):
        """
        :rtype: int
        """
        ret = self.stack.pop()
        if ret.right is not None:
            self._push_left(ret.right)
        return ret.val

    def _push_left(self, node):
        while node is not None:
            self.stack.append(node)
            node = node.left

# Your BSTIterator will be called like this:
# i, v = BSTIterator(root), []
# while i.hasNext(): v.append(i.next())
```

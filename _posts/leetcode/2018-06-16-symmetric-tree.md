---
layout: "post"
title: "Leetcode 101: Symmetric Tree"
date: "2018-06-16 16:13"
tags:
  - Leetcode
---

# Question
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

But the following [1,2,2,null,3,null,3] is not:

```
    1
   / \
  2   2
   \   \
   3    3
```

Note:

Bonus points if you could solve it both recursively and iteratively.

# Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if root is None:
            return True
        if root.left is None and root.right is None:
            return True
        elif root.left is not None and root.right is not None:
            return self.isMirror(root.left, root.right)
        else:
            return False

    def isMirror(self, left_root, right_root):
        # val
        if left_root.val != right_root.val:
            return False

        # One side
        if left_root.left is not None and right_root.right is not None:
            valid = self.isMirror(left_root.left, right_root.right)
            if not valid:
                return False
        elif (left_root.left is not None and right_root.right is None) or \
        (left_root.left is None and right_root.right is not None):
            return False

        # The other side
        if left_root.right is not None and right_root.left is not None:
            valid = self.isMirror(left_root.right, right_root.left)
            if not valid:
                return False
        elif (left_root.right is not None and right_root.left is None) or \
        (left_root.right is None and right_root.left is not None):
            return False

        return True

```

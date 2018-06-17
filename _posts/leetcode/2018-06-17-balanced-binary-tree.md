---
layout: "post"
title: "Leetcode 110: Balanced Binary Tree"
date: "2018-06-17 14:34"
tags:
  - Leetcode
---

# Question
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Example 1:

Given the following tree `[3,9,20,null,null,15,7]`:

```
    3
   / \
  9  20
    /  \
   15   7
```

Return true.

Example 2:

Given the following tree `[1,2,2,3,3,null,null,4,4]`:

```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
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
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        return not (self.height(root) == -1)

    def height(self, root):
        """
        :type root: TreeNode
        :rtype: int, -1 if not balanced
        """
        if root is None:
            return 0
        else:
            left = self.height(root.left)
            if left == -1:
                return -1
            right = self.height(root.right)
            if right == -1:
                return -1

            if -1 <= left - right <= 1:
                return max(left, right) + 1
            else:
                return -1
```

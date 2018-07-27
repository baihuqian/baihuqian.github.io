---
layout: "post"
title: "Leetcode 250: Count Univalue Subtrees"
date: "2018-07-26 20:22"
tags:
  - Leetcode
---

# Question
Given a binary tree, count the number of uni-value subtrees.

A Uni-value subtree means all nodes of the subtree have the same value.

Example :

```
Input:  root = [5,1,5,5,5,null,5]

              5
             / \
            1   5
           / \   \
          5   5   5

Output: 4
```

# Solution
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def countUnivalSubtrees(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root is None:
            return 0
        else:
            return self.helper(root)[0]

    def helper(self, root):
        if root.left is None and root.right is None:
            return (1, True)
        elif root.left is not None and root.right is None:
            subTreeCount, isUnvalSubTree = self.helper(root.left)
            isMatch = root.val == root.left.val and isUnvalSubTree
            return (subTreeCount + (1 if isMatch else 0), isMatch)
        elif root.left is None and root.right is not None:
            subTreeCount, isUnvalSubTree = self.helper(root.right)
            isMatch = root.val == root.right.val and isUnvalSubTree
            return (subTreeCount + (1 if isMatch else 0), isMatch)
        else:
            subTreeCountLeft, isUnvalSubTreeLeft = self.helper(root.left)
            subTreeCountRight, isUnvalSubTreeRight = self.helper(root.right)
            isMatch = root.val == root.left.val == root.right.val and isUnvalSubTreeLeft and isUnvalSubTreeRight
            return (subTreeCountLeft + subTreeCountRight + (1 if isMatch else 0), isMatch)
```

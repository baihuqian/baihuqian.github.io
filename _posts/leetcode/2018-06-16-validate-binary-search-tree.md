---
layout: post
title: 'Leetcode 98: Validate Binary Search Tree'
date: '2018-06-16 16:01'
tags:
  - Leetcode
published: true
---

# Question
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys less than the node's key.
* The right subtree of a node contains only nodes with keys greater than the node's key.
* Both the left and right subtrees must also be binary search trees.

Example 1:

```
Input:
    2
   / \
  1   3
Output: true
```

Example 2:
```
    5
   / \
  1   4
     / \
    3   6
Output: false
Explanation: The input is: [5,1,4,null,null,3,6]. The root node's value
             is 5 but its right child's value is 4.
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
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if root is None:
            return True
        return self.validate(root, None, None)

    def validate(self, root, min_val, max_val):
        if min_val is not None and root.val <= min_val:
            return False
        if max_val is not None and root.val >= max_val:
            return False
        if root.left is not None:
            if max_val is None:
                new_max_val = root.val
            else:
                new_max_val = min(root.val, max_val)
            isValid = self.validate(root.left, min_val, new_max_val)
            if not isValid:
                return False
        if root.right is not None:
            if min_val is None:
                new_min_val = root.val
            else:
                new_min_val = max(root.val, min_val)
            isValid = self.validate(root.right, new_min_val, max_val)
            if not isValid:
                return False
        return True
```

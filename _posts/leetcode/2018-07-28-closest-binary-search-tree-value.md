---
layout: "post"
title: "Leetcode 270: Closest Binary Search Tree Value"
date: "2018-07-28 22:41"
tags:
  - Leetcode
---

# Question
Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

Note:

* Given target value is a floating point.
* You are guaranteed to have only one unique value in the BST that is closest to the target.

Example:
```
Input: root = [4,2,5,1,3], target = 3.714286

    4
   / \
  2   5
 / \
1   3

Output: 4
```

# Solution
Walk the BST and find the maximum number smaller than the target and minimum number larger than the target.
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def closestValue(self, root, target):
        """
        :type root: TreeNode
        :type target: float
        :rtype: int
        """
        self.min_val = float('-inf')
        self.max_val = float('inf')
        self.closest_val = root.val
        self.__walk_tree(root, target)
        return self.closest_val

    def __walk_tree(self, root, target):
        if root is None:
            if target - self.min_val > self.max_val - target:
                self.closest_val = self.max_val
            else:
                self.closest_val = self.min_val
        elif target > root.val:
            self.min_val = root.val
            self.__walk_tree(root.right, target)
        elif target < root.val:
            self.max_val = root.val
            self.__walk_tree(root.left, target)
        else:
            self.closest_val = root.val
```

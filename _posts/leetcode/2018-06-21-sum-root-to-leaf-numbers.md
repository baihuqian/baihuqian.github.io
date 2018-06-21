---
layout: "post"
title: "Leetcode 129: Sum Root to Leaf Numbers"
date: "2018-06-21 20:33"
tags:
  - Leetcode
---

# Question
Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

Note: A leaf is a node with no children.

Example:
```
Input: [1,2,3]
    1
   / \
  2   3
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```

Example 2:
```
Input: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
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
    def sumNumbers(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root is None:
            return 0
        return self.sumNode(0, root)

    def sumNode(self, sums, node):
        if node.left is None and node.right is None:
            return sums + node.val
        if node.left is not None:
            node.left.val = node.val * 10 + node.left.val
            sums = self.sumNode(sums, node.left)
        if node.right is not None:
            node.right.val = node.val * 10 + node.right.val
            sums = self.sumNode(sums, node.right)
        return sums
```

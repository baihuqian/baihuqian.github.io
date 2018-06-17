---
layout: post
title: 'Leetcode 95: Unique Binary Search Trees II'
date: '2018-06-16 15:48'
tags:
  - Leetcode
---

# Question
Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1 ... n.

Example:
```
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
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
    def generateTrees(self, n):
        """
        :type n: int
        :rtype: List[TreeNode]
        """
        if n == 0:
            return []
        return self.generateTree(1, n + 1)

    def generateTree(self, low, high):
        n = high - low
        if n == 0:
            return [None]
        elif n == 1:
            return [TreeNode(low)]
        else:
            res = []
            for root in range(low, high):
                left = self.generateTree(low, root)
                right = self.generateTree(root + 1, high)

                for l in left:
                    for r in right:
                        rNode = TreeNode(root)
                        rNode.left = l
                        rNode.right = r
                        res.append(rNode)
            return res
```

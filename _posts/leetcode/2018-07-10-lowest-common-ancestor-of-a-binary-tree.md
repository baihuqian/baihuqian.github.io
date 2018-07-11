---
layout: post
title: 'Leetcode 236: Lowest Common Ancestor of a Binary Tree'
date: '2018-07-10 21:48'
tags:
  - Leetcode
  - Review
---

# Question
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

Given the following binary tree:  root = [3,5,1,6,2,0,8,null,null,7,4]

```
        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
```

Example 1:

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of of nodes 5 and 1 is 3.
```

Example 2:

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself
             according to the LCA definition.
```

Note:

* All of the nodes' values will be unique.
* p and q are different and both values will exist in the binary tree.

# Solution
The question becomes finding the smallest subtree that contains both p and q and returning its root. A subtree contains a node is either a node in its left tree, right tree, or its root. Because of the recursive call, we can store the first occurrence of such tree when both p and q exists, and it is the LCA.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        self.lca = None
        self.contains(root, p, q)
        return self.lca

    def contains(self, root, p, q):
        if root is None:
            return False, False

        left_p, left_q = self.contains(root.left, p, q)

        right_p, right_q = self.contains(root.right, p, q)    

        if self.lca is not None:
            return True, True

        contains_p = root.val == p.val or left_p or right_p
        contains_q = root.val == q.val or left_q or right_q

        if contains_p and contains_q:
            self.lca = root

        return contains_p, contains_q
```

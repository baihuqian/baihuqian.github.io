---
layout: "post"
title: "Leetcode 366: Find Leaves of Binary Tree"
date: "2018-08-13 21:42"
tags:
  - Leetcode
---

# Question
Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

Example:
```
Given binary tree
          1
         / \
        2   3
       / \     
      4   5    
```
Returns `[4, 5, 3], [2], [1]`.

Explanation:
1. Removing the leaves `[4, 5, 3]` would result in this tree:
```
          1
         /
        2          
```

2. Now removing the leaf `[2]` would result in this tree:
```
          1          
```
3. Now removing the leaf `[1]` would result in the empty tree:
```
          []         
```

Returns `[4, 5, 3], [2], [1]`.

# Solution
```python
from collections import defaultdict
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def __init__(self):
        self.nodedict = defaultdict(list)

    def findLeaves(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if root is None:
            return []
        res = []
        for lvl in range(1, self.getLevel(root) + 1):
            res.append(self.nodedict[lvl])
        return res

    def getLevel(self, root):
        if root.left is None and root.right is None:
            lvl = 1
        else:
            l, r = 0, 0
            if root.left is not None:
                l = self.getLevel(root.left)
            if root.right is not None:
                r = self.getLevel(root.right)
            lvl = max(l, r) + 1
        self.nodedict[lvl].append(root.val)
        return lvl
```

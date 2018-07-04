---
layout: "post"
title: "Leetcode 199: Binary Tree Right Side View"
date: "2018-07-03 20:18"
tags:
  - Leetcode
---

# Question
Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

Example:

```
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
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
    def rightSideView(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if root is None:
            return []

        level = []
        res = []
        level.append(root)
        while len(level) != 0:
            res.append(level[-1].val)
            new_level = []
            for n in level:
                if n.left is not None:
                    new_level.append(n.left)
                if n.right is not None:
                    new_level.append(n.right)
            level = new_level

        return res
```

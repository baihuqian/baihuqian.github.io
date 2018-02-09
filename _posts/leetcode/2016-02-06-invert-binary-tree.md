---
layout: "post"
title: "Leetcode 226: Invert Binary Tree"
date: "2016-02-06 23:56"
tags: Leetcode
---

# Question

Invert a binary tree.
```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

to

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
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
    def invertTree(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        if root is None:
            return None

        node_list = [root]
        while True:
            try:
                n = node_list[-1]
                del node_list[-1]
                l = n.left
                r = n.right
                n.left = r
                n.right = l
                if l is not None:
                    node_list.append(l)
                if r is not None:
                    node_list.append(r)

            except IndexError:
                break
        return root
```

---
layout: "post"
title: "Leetcode 156: Binary Tree Upside Down"
date: "2018-06-24 21:35"
tags:
  - Leetcode
---

# Question
Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.

Example:
```
Input: [1,2,3,4,5]

    1
   / \
  2   3
 / \
4   5

Output: return the root of the binary tree [4,5,2,#,#,3,1]

   4
  / \
 5   2
    / \
   3   1  
```

Clarification:

Confused what `[4,5,2,#,#,3,1]` means? Read more below on how binary tree is serialized on OJ.

The serialization of a binary tree follows a level order traversal, where '#' signifies a path terminator where no node exists below.

Here's an example:

```
   1
  / \
 2   3
    /
   4
    \
     5
```

The above binary tree is serialized as `[1,2,3,#,#,4,#,#,5]`.

# Solution
Process level by level.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def upsideDownBinaryTree(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        if root is None:
            return None
        if root.left is None:
            return root

        new_root, prev_root = None, None
        while root.left is not None:
            if prev_root is None:
                new_root = TreeNode(root.left.val)
                new_root.left = root.right
                new_root.right = TreeNode(root.val)
            else:
                new_root = TreeNode(root.left.val)
                new_root.left = root.right
                new_root.right = prev_root
            prev_root = new_root
            root = root.left

        return new_root
```

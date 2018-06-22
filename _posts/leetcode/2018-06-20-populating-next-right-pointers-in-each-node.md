---
layout: "post"
title: "Leetcode 116: Populating Next Right Pointers in Each Node"
date: "2018-06-20 19:38"
tags:
  - Leetcode
---

# Question
Given a binary tree
```
struct TreeLinkNode {
  TreeLinkNode *left;
  TreeLinkNode *right;
  TreeLinkNode *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

* You may only use constant extra space.
* Recursive approach is fine, implicit stack space does not count as extra space for this problem.
* You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).
*
Example:

Given the following perfect binary tree,
```
     1
   /  \
  2    3
 / \  / \
4  5  6  7
```

After calling your function, the tree should look like:
```
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \  / \
4->5->6->7 -> NULL
```

# Solution
```python
# Definition for binary tree with next pointer.
# class TreeLinkNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
#         self.next = None

class Solution:
    # @param root, a tree link node
    # @return nothing
    def connect(self, root):
        self.connect_internal(root, None)

    def connect_internal(self, node, prev):
        if node is None:
            return
        if node.left is None:
            return
        else:
            if prev is not None:
                prev.right.next = node.left
                self.connect_internal(node.left, prev.right)
            else:
                self.connect_internal(node.left, None)

            node.left.next = node.right
            self.connect_internal(node.right, node.left)

```

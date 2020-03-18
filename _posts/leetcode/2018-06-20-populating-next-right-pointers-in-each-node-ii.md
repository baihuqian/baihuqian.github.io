---
layout: post
title: 'Leetcode 117: Populating Next Right Pointers in Each Node II'
date: '2018-06-20 19:50'
tags:
  - Leetcode
published: true
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

Example:

Given the following binary tree,
```
     1
   /  \
  2    3
 / \    \
4   5    7
```

After calling your function, the tree should look like:
```
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \    \
4-> 5 -> 7 -> NULL
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
        if root is None or (root.left is None and root.right is None):
            return
        head = root
        while True:
            head = self.connect_next_level(head)
            if head is None:
                break


    def connect_next_level(self, head):
        next_head = None
        node = None
        while head is not None:
            if head.left is not None:
                if node is None:
                    node = head.left
                    next_head = node
                else:
                    node.next = head.left
                    node = head.left
            if head.right is not None:
                if node is None:
                    node = head.right
                    next_head = node
                else:
                    node.next = head.right
                    node = head.right
            head = head.next
        return next_head
```

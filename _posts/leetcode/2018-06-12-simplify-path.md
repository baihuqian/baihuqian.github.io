---
layout: post
title: 'Leetcode 71: Simplify Path'
date: '2018-06-12 23:05'
tags:
  - Leetcode
---

# Question
Given an absolute path for a file (Unix-style), simplify it.

For example,
path = `"/home/"`, => "`/home"`
path = `"/a/./b/../../c/"``, => `"/c"`

Corner Cases:

* Did you consider the case where path = `"/../"`? In this case, you should return `"/"`.
* Another corner case is the path might contain multiple slashes `'/'` together, such as `"/home//foo/"`. In this case, you should ignore redundant slashes and return `"/home/foo"`.

# Solution
```python
class Solution:
    def simplifyPath(self, path):
        """
        :type path: str
        :rtype: str
        """
        if len(path) == 0:
            return "/"

        nodes = path.split('/')
        res = list()
        prefix = ''
        if nodes[0] == '':
            prefix = '/'
            nodes[1:]

        for node in nodes:
            if node == '..':
                if len(res) > 0:
                    res.pop()
            elif node != '.' and node != '':
                res.append(node)

        return prefix + '/'.join(res)
            
```

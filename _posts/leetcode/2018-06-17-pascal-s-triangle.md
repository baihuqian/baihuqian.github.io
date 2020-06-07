---
layout: post
title: 'Leetcode 118: Pascal''s Triangle'
date: '2018-06-17 22:25'
tags:
  - Leetcode
published: true
---

# Question
Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.

![Pascal Triangle](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)

In Pascal's triangle, each number is the sum of the two numbers directly above it.

Example:
```
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

# Solution
```python
class Solution:
    def generate(self, numRows):
        """
        :type numRows: int
        :rtype: List[List[int]]
        """
        if numRows == 0:
            return []

        ret = []
        prev_row = []
        for i in range(1, numRows + 1):
            row = [1 for _ in range(i)]
            if i >= 3:
                for j in range(1, i - 1):
                    row[j] = prev_row[j - 1] + prev_row[j]
            ret.append(row)
            prev_row = row

        return ret

```

---
layout: "post"
title: "Leetcode 251: Flatten 2D Vector"
date: "2018-07-26 20:33"
tags:
  - Leetcode
---

# Question
Implement an iterator to flatten a 2d vector.

Example:

```
Input: 2d vector =
[
  [1,2],
  [3],
  [4,5,6]
]
Output: [1,2,3,4,5,6]
Explanation: By calling next repeatedly until hasNext returns false,
             the order of elements returned by next should be: [1,2,3,4,5,6].
```

# Question
```python
class Vector2D(object):

    def __init__(self, vec2d):
        """
        Initialize your data structure here.
        :type vec2d: List[List[int]]
        """
        self.vecCounter = 0
        self.listCounter = 0
        self.vec2d = [v for v in vec2d if len(v) > 0]

    def next(self):
        """
        :rtype: int
        """
        retVal = self.vec2d[self.vecCounter][self.listCounter]
        if self.listCounter == len(self.vec2d[self.vecCounter]) - 1:
            self.vecCounter += 1
            self.listCounter = 0
        else:
            self.listCounter += 1
        return retVal

    def hasNext(self):
        """
        :rtype: bool
        """
        return self.vecCounter < len(self.vec2d)


# Your Vector2D object will be instantiated and called as such:
# i, v = Vector2D(vec2d), []
# while i.hasNext(): v.append(i.next())
```

---
layout: "post"
title: "Leetcode 281: Zigzag Iterator"
date: "2018-07-30 21:03"
tags:
  - Leetcode
---

Given two 1d vectors, implement an iterator to return their elements alternately.

Example:

```
Input:
v1 = [1,2]
v2 = [3,4,5,6]

Output: [1,3,2,4,5,6]

Explanation: By calling next repeatedly until hasNext returns false,
             the order of elements returned by next should be: [1,3,2,4,5,6].
```

Follow up: What if you are given k 1d vectors? How well can your code be extended to such cases?

Clarification for the follow up question:

The "Zigzag" order is not clearly defined and is ambiguous for k > 2 cases. If "Zigzag" does not look right to you, replace "Zigzag" with "Cyclic". For example:

```
Input:
[1,2,3]
[4,5,6,7]
[8,9]

Output: [1,4,8,2,5,9,3,6,7].

```

# Solution
```python
class ZigzagIterator(object):

    def __init__(self, v1, v2):
        """
        Initialize your data structure here.
        :type v1: List[int]
        :type v2: List[int]
        """
        self.lists = list()
        if len(v1) > 0:
            self.lists.append(v1)
        if len(v2) > 0:
            self.lists.append(v2)

        self.numLists = len(self.lists)
        self.counters = [0] * self.numLists
        self.listCounter = 0

    def next(self):
        """
        :rtype: int
        """
        res = self.lists[self.listCounter][self.counters[self.listCounter]]
        if self.counters[self.listCounter] == len(self.lists[self.listCounter]) - 1:
            del self.counters[self.listCounter]
            del self.lists[self.listCounter]
            self.numLists -= 1
        else:
            self.counters[self.listCounter] += 1
            self.listCounter += 1
        if self.listCounter == self.numLists:
            self.listCounter = 0
        return res

    def hasNext(self):
        """
        :rtype: bool
        """
        return self.numLists > 0


# Your ZigzagIterator object will be instantiated and called as such:
# i, v = ZigzagIterator(v1, v2), []
# while i.hasNext(): v.append(i.next())
```

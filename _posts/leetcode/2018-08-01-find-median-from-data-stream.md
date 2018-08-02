---
layout: "post"
title: "Leetcode 295: Find Median from Data Stream"
date: "2018-08-01 20:05"
tags:
  - Leetcode
  - Hard
  - Review
---

# Question
Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

For example,
`[2,3,4]`, the median is `3`

`[2,3]`, the median is `(2 + 3) / 2 = 2.5`

Design a data structure that supports the following two operations:

* `void addNum(int num)` - Add a integer number from the data stream to the data structure.
* `double findMedian()` - Return the median of all elements so far.
Example:

```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3)
findMedian() -> 2
```

# Solution - Two Heaps
Concretely, one can infer two things:

1. If we could maintain direct access to median elements at all times, then finding the median would take a constant amount of time.
1. If we could find a reasonably fast way of adding numbers to our containers, additional penalties incurred could be lessened.

But perhaps the most important insight, which is not readily observable, is the fact that we only need a consistent way to access the median elements. Keeping the entire input sorted is not a requirement.

As it turns out there are two data structures for the job:

1. Heaps (or Priority Queues)
2. Self-balancing Binary Search Trees (like AVL Trees)

Heaps are a natural ingredient for this dish! Adding elements to them take logarithmic order of time. They also give direct access to the maximal/minimal elements in a group.

If we could maintain two heaps in the following way:

* A max-heap to store the smaller half of the input numbers
* A min-heap to store the larger half of the input numbers

This gives access to median values in the input: they comprise the top of the heaps, if the following conditions are met:

* Both the heaps are balanced (or nearly balanced)
* The max-heap contains all the smaller numbers while the min-heap contains all the larger numbers

then we can say that:

* All the numbers in the max-heap are smaller or equal to the top element of the max-heap (let's call it x)
* All the numbers in the min-heap are larger or equal to the top element of the min-heap (let's call it y)

Then x and/or y are smaller than (or equal to) almost half of the elements and larger than (or equal to) the other half. That is the definition of median elements.

This leads us to a huge point of pain in this approach: balancing the two heaps! We can do the following:
1. Compare the next number of y. If it is larger or equal to y, then add it to the max-heap; otherwise add it to the min heap.
2. Move elements between two heaps until the size of the max-heap is smaller than 1 plus the size of the min-heap.

```python
from heapq import *
class MedianFinder(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.lo = list() # max heap
        self.hi = list() # min heap


    def addNum(self, num):
        """
        :type num: int
        :rtype: void
        """
        if len(self.hi) > 0 and num >= self.hi[0]:
            heappush(self.hi, num)
        else:
            heappush(self.lo, -num)

        # Rebalance
        while len(self.lo) > len(self.hi) + 1:
            heappush(self.hi, -heappop(self.lo))
        while len(self.hi) > len(self.lo):
            heappush(self.lo, -heappop(self.hi))


    def findMedian(self):
        """
        :rtype: float
        """
        if len(self.lo) == len(self.hi):
            return float(self.hi[0] - self.lo[0]) / 2
        else:
            return float(-self.lo[0])



# Your MedianFinder object will be instantiated and called as such:
# obj = MedianFinder()
# obj.addNum(num)
# param_2 = obj.findMedian()
```

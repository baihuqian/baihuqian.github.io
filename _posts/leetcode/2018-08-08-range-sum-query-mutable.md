---
layout: post
title: 'Leetcode 307: Range Sum Query - Mutable'
date: '2018-08-08 23:20'
tags:
  - Leetcode
  - Review
published: true
---

# Question
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.

Example:

```
Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
```

Note:

1. The array is only modifiable by the update function.
1. You may assume the number of calls to update and sumRange function is distributed evenly.

# Sqrt Decomposition Solution
The idea is to split the array in blocks with length of $$\sqrt{n}$$. Then we calculate the sum of each block and store it in auxiliary memory `b`. To query `RSQ(i, j)`, we will add the sums of all the blocks lying inside and those that partially overlap with range $$[i \ldots j]$$.

Time complexity : $$O(n)$$ - preprocessing, $$O(\sqrt{n})$$ - range sum query, $$O(1)$$ - update query

For range sum query in the worst-case scenario we have to sum approximately $$3 \sqrt{n}$$ elements. In this case the range includes $$\sqrt{n} - 2$$ blocks, which total sum costs $$\sqrt{n} - 2$$ operations. In addition to this we have to add the sum of the two boundary blocks. This takes another $$2 (\sqrt{n} - 1)$$ operations. The total amount of operations is around $$3 \sqrt{n}$$.

Space complexity : $$O(\sqrt{n})$$.

We need additional $$O(\sqrt{n})$$ memory to store all block sums.

```python
from math import sqrt

class NumArray(object):

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        self.nums = nums
        self.block_size = int(sqrt(len(nums)))
        self.blocks = []
        if len(self.nums) > 0:
            for i in range(self._block_id(len(nums) - 1) + 1):
                self.blocks.append(sum(self.nums[i * self.block_size:(i + 1) * self.block_size]))

    def _block_id(self, idx):
        return idx // self.block_size

    def update(self, i, val):
        """
        :type i: int
        :type val: int
        :rtype: void
        """
        self.blocks[self._block_id(i)] += val - self.nums[i]
        self.nums[i] = val


    def sumRange(self, i, j):
        """
        :type i: int
        :type j: int
        :rtype: int
        """
        if j - i + 1 < self.block_size:
            return sum(self.nums[i:j + 1])
        start_block = self._block_id(i) + (1 if i % self.block_size != 0 else 0)
        end_block = self._block_id(j) - (1 if (j + 1) % self.block_size != 0 else 0)
        return sum(self.blocks[start_block:end_block + 1]) + \
               sum(self.nums[i: self.block_size * start_block]) + \
               sum(self.nums[self.block_size * (end_block + 1): j + 1])


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)
```

# Segment Tree Solution
```python
class NumArray(object):

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        if self.n > 0:
            for i, n in enumerate(nums):
                self.tree[self.n + i] = n
            for i in range(self.n - 1, 0, -1):
                self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]


    def update(self, i, val):
        """
        :type i: int
        :type val: int
        :rtype: void
        """
        i += self.n
        self.tree[i] = val
        i //= 2
        while i > 0:
            self.tree[i] = self.tree[i * 2] + self.tree[i * 2 + 1]
            i //= 2


    def sumRange(self, i, j):
        """
        :type i: int
        :type j: int
        :rtype: int
        """
        i += self.n
        j += self.n
        s = 0
        while i <= j:
            if i % 2 == 1:
                s += self.tree[i]
                i += 1
            if j % 2 == 0:
                s += self.tree[j]
                j -= 1
            i //= 2
            j //= 2

        return s



# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# obj.update(i,val)
# param_2 = obj.sumRange(i,j)
```

---
layout: "post"
title: "Leetcode 11: Container With Most Water"
date: "2018-05-18 22:09"
tags:
  - Leetcode
---

# Question
Given n non-negative integers $$a_1$$, $$a_2$$, ..., $$a_n$$, where each represents a point at coordinate $$(i, a_i)$$. n vertical lines are drawn such that the two endpoints of line $$i$$ is at $$(i, a_i)$$ and $$(i, 0)$$. Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

# O(nlogn) Solution
For each height, find the farthest line which height equal or larger than this, and compute the area. This is done by sort the lines by height in reverse order, and traverse from largest to smallest by recording the leftmost and rightmost $$i$$.

```python
class Solution:
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        height = list(enumerate(height))
        height.sort(key=lambda x: x[1], reverse=True)

        maxArea = 0
        left, right = (height[0][0], height[1][0]) if height[0][0] < height[1][0] else (height[1][0], height[0][0])
        for idx in range(1, len(height)):
            i, h = height[idx]
            if i <= left:
                area = h * (right - i)
                left = i
            elif i >= right:
                area = h * (i - left)
                right = i
            else:
                area = h * max(right - i, i - left)
            if area > maxArea:
                maxArea = area

        return maxArea


```

# O(n) Solution
We take two pointers, one at the beginning and one at the end of the array constituting the length of the lines. Further, we maintain a variable maxarea to store the maximum area obtained till now. At every step, we find out the area formed between them, update maxarea and move the pointer pointing to the shorter line towards the other end by one step.

Here's another way to see what happens in a matrix representation:

Draw a matrix where the row is the first line, and the column is the second line. For example, say `n=6`.

In the figures below, `x` means we don't need to compute the volume for that case: (1) On the diagonal, the two lines are overlapped; (2) The lower left triangle area of the matrix is symmetric to the upper right area.

We start by computing the volume at `(1,6)`, denoted by o. Now if the left line is shorter than the right line, then all the elements left to `(1,6)` on the first row have smaller volume, so we don't need to compute those cases (crossed by `---`).

```
  1 2 3 4 5 6
1 x ------- o
2 x x
3 x x x
4 x x x x
5 x x x x x
6 x x x x x x
```

Next we move the left line and compute `(2,6)`. Now if the right line is shorter, all cases below `(2,6)` are eliminated.
```
  1 2 3 4 5 6
1 x ------- o
2 x x       o
3 x x x     |
4 x x x x   |
5 x x x x x |
6 x x x x x x
```
And no matter how this o path goes, we end up only need to find the max value on this path, which contains `n-1` cases.
```
  1 2 3 4 5 6
1 x ------- o
2 x x - o o o
3 x x x o | |
4 x x x x | |
5 x x x x x |
6 x x x x x x
```

```python
class Solution:
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        maxArea = 0
        left, right = 0, len(height) - 1

        while True:
            area = (right - left) * min(height[right], height[left])
            if area > maxArea:
                maxArea = area
            if height[right] >= height[left]:
                left += 1
            else:
                right -= 1
            if left == right:
                break

        return maxArea
```

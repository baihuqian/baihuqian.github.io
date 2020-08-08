---
layout: "post"
title: "Leetcode 85: Maximal Rectangle"
date: "2018-08-16 21:45"
tags:
  - Leetcode
  - Review
  - Hard
---

# Question
Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

Example:

```
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
```

# Solution
Reuse the $$O(n)$$ solution from [Largest Rectangle in Histogram]({{ site.baseurl }}{% link _posts/leetcode/2018-08-15-largest-rectangle-in-histogram.md %}). Maintain an array of size of width of the array that stores the height of 1's at each index at row `i`. Therefore, we can run the stack solution to find the largest rectangle at row `i`. Then for next row, if an index `j` is '1', then we increment the height of the 1s; otherwise we reset the height to 0.

The time complexity if $$O(mn)$$.

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if(matrix.length == 0 || matrix[0].length == 0)
            return 0;

        int x = matrix.length, y = matrix[0].length;
        int maxArea = 0;
        int[] h = new int[y];
        Arrays.fill(h, 0);

        Stack<Integer> stack = new Stack<>();
        stack.push(-1);
        for(int i = 0; i < x; i++) {
            for(int j = 0; j < y; j++) {
                if(matrix[i][j] == '1')
                    h[j]++;
                else
                    h[j] = 0;
                while(stack.peek() != -1 && h[stack.peek()] > h[j]) {
                    maxArea = Math.max(maxArea, h[stack.pop()] * (j - stack.peek() - 1));
                }
                stack.push(j);
            }
            while(stack.peek() != -1) {
                maxArea = Math.max(maxArea, h[stack.pop()] * (y - stack.peek() - 1));
            }
        }
        return maxArea;
    }
}
```

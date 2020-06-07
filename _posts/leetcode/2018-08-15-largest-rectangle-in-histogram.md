---
layout: "post"
title: "Leetcode 84: Largest Rectangle in Histogram"
date: "2018-08-15 22:32"
tags:
  - Leetcode
  - Review
  - Hard
---

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![Histogram](https://leetcode.com/static/images/problemset/histogram.png)

Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].

![Histogram Area](https://leetcode.com/static/images/problemset/histogram_area.png)

The largest rectangle is shown in the shaded area, which has area = 10 unit.

Example:

```
Input: [2,1,5,6,2,3]
Output: 10
```

# $$O(nlogn)$$ Divide-and-Conquer Solution
This approach relies on the observation that the rectangle with maximum area will be the maximum of:

1. The widest possible rectangle with height equal to the height of the shortest bar.
1. The largest rectangle confined to the left of the shortest bar(subproblem).
1. The largest rectangle confined to the right of the shortest bar(subproblem).

```java
class Solution {
    int maxArea;
    public int largestRectangleArea(int[] heights) {
        maxArea = 0;
        if(heights.length > 0)
            largestRectangleArea(heights, 0, heights.length - 1);
        return maxArea;
    }

    public void largestRectangleArea(int[] heights, int left, int right) {
        if(left == right) {
            maxArea = Math.max(heights[left], maxArea);
        } else {
            int minLoc = findMin(heights, left, right);
            maxArea = Math.max(heights[minLoc] * (right - left + 1), maxArea);
            if(minLoc != left)
                largestRectangleArea(heights, left, minLoc - 1);
            if(minLoc != right)
                largestRectangleArea(heights, minLoc + 1, right);
        }
    }

    public int findMin(int[] heights, int left, int right) {
        int min = Integer.MAX_VALUE;
        int idx = -1;

        for(int i = left; i <= right; i++) {
            if(heights[i] < min) {
                min = heights[i];
                idx = i;
            }
        }
        return idx;
    }
}
```

# $$O(n)$$ Solution
In this approach, we maintain a stack of indices of increasing height of histograms. Initially, we push a `-1` onto the stack to mark the end. We start with the leftmost bar and keep pushing the current bar's index onto the stack until we get two successive numbers in descending order, i.e. until we get `a[i]` where `a[i] < a[i - 1]`. Now, we start popping the numbers from the stack until we hit a number `stack[j]` on the stack such that `a[stack[j]]<=a[i]`.

Every time we pop, we find out the area of rectangle formed between the current bar `i` and `stack[top−1]`, exclusive, as the bar at `stack[top]` will be higher than both of them, but other bars in between will be higher. So we can use the current element as the height of the rectangle and the difference between the the current element's index pointed to in the original array and the element `stack[top−1]−1` as the width i.e. if we pop an element `stack[top]` and `i` is the current index to which we are pointing in the original array, the current area of the rectangle will be considered as: `(i−stack[top−1]−1)* a[stack[top]]`.

Further, if we reach the end of the array, we ensure that bars on the right of each element in the stack is at least as high as the bar at each element. At every pop, we use the following equation to find the area from `stack[top - 1] to the rightmost`: `(a.length-1−stack[top−1])*a[stack[top]]`, where `stack[top]` refers to the element just popped. Thus, we can get the area of the of the largest rectangle by comparing the new area found every time.

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int maxArea = 0;
        Stack<Integer> stack = new Stack<>();
        stack.push(-1);

        int i = 0;
        while(i < heights.length) {
            while(stack.peek() != -1 && heights[stack.peek()] > heights[i]) {
                maxArea = Math.max(maxArea, heights[stack.pop()] * (i - stack.peek() - 1));
            }
            stack.push(i++);
        }
        while(stack.peek() != -1) {
            maxArea = Math.max(maxArea, heights[stack.pop()] * (heights.length - stack.peek() - 1));
        }
        return maxArea;
    }
}
```

---
layout: "post"
title: "Leetcode 356: Line Reflection"
date: "2018-08-12 18:48"
tags:
  - Leetcode
  - Review
---

# Question
Given n points on a 2D plane, find if there is such a line parallel to y-axis that reflect the given points.

Example 1:
```
Input: [[1,1],[-1,1]]
Output: true
```

Example 2:
```
Input: [[1,1],[-1,-1]]
Output: false
```

Follow up:

Could you do better than $$O(n^2)$$ ?

# Solution
Like 2Sum, we put all points into a set, and find the corresponding point for each point in the set. The x value for a point and its corresponding point sums to a fixed value, which can be obtained by summing the min and max of x value.

To put points into set, we can concatenate two integers into long using ByteBuffer.

```java
import java.nio.ByteBuffer;

class Solution {
    public boolean isReflected(int[][] points) {
        if (points.length <= 1) return true;

        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        Set<Long> pointSet = new HashSet<>();
        for(int[] p: points) {
            min = Math.min(min, p[0]);
            max = Math.max(max, p[0]);
            pointSet.add(pointToLong(p[0], p[1]));
        }
        int sum = min + max;
        for(int[] p: points) {
            if(!pointSet.contains(pointToLong(sum - p[0], p[1])))
                return false;
        }
        return true;
    }

    public long pointToLong(int x, int y) {
        ByteBuffer bb = ByteBuffer.allocate(8);
        bb.putInt(x);
        bb.putInt(y);
        return bb.getLong(0);
    }
}
```

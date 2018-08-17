---
layout: "post"
title: "Leetcode 370: Range Addition"
date: "2018-08-16 23:13"
tags:
  - Leetcode
  - Review
---

# Question
Assume you have an array of length n initialized with all 0's and are given k update operations.

Each operation is represented as a triplet: [startIndex, endIndex, inc] which increments each element of subarray A[startIndex ... endIndex] (startIndex and endIndex inclusive) with inc.

Return the modified array after all k operations were executed.

Example:

```
Given:

    length = 5,
    updates = [
        [1,  3,  2],
        [2,  4,  3],
        [0,  2, -2]
    ]

Output:

    [-2, 0, 3, 5, 3]
```

Explanation:

```
Initial state:
[ 0, 0, 0, 0, 0 ]

After applying operation [1, 3, 2]:
[ 0, 2, 2, 2, 0 ]

After applying operation [2, 4, 3]:
[ 0, 2, 5, 5, 3 ]

After applying operation [0, 2, -2]:
[-2, 0, 3, 5, 3 ]
```

# Solution
Intuition:
* There is only one read query on the entire range, and it occurs at the end of all update queries. Additionally, the order of processing update queries is irrelevant. Therefore, we don't have to process the entire range until the end of the updates.
* Cumulative sums operations apply the effects of past elements to the future elements in the sequence.

Therefore, for every (start, end, val) updates, we only need to do two operations:
* `arr[start] += val`;
* `arr[end + 1] -= val`;

At the end, we apply cumulative sum to the array: `array[i] += array[i - 1]`.

For each update query $$(start, end, val)$$ on the array `arr`, the goal is to achieve the result: $$arr_i = arr_i + val \quad \forall \quad i \in [start, end]$$.

Applying the final transformation, ensures two things:
1. It carries over the $$+val$$ increment over to every element $$arr_i \; \forall \; i \ge start$$.
1. It carries over the $$−val$$ increment (equivalently, a $$+val$$ decrement) over to every element $$arr_j \; \forall \; j \gt end $$.

```java
class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {
        int[] arr = new int[length];
        Arrays.fill(arr, 0);

        for(int i = 0; i < updates.length; i++) {
            arr[updates[i][0]] += updates[i][2];
            if(updates[i][1] < length - 1)
                arr[updates[i][1] + 1] -= updates[i][2];
        }
        for(int i = 1; i < length; i++) {
            arr[i] += arr[i - 1];
        }

        return arr;
    }
}
```

---
layout: "post"
title: "Leetcode 384: Shuffle an Array"
date: "2018-08-27 20:43"
tags:
  - Leetcode
---

# Question
Shuffle a set of numbers without duplicates.

Example:

```
// Init an array with set 1, 2, and 3.
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// Shuffle the array [1,2,3] and return its result. Any permutation of [1,2,3] must equally likely to be returned.
solution.shuffle();

// Resets the array back to its original configuration [1,2,3].
solution.reset();

// Returns the random shuffling of array [1,2,3].
solution.shuffle();
```

# Solution
The Fisher-Yates algorithm. On each iteration of the algorithm, we generate a random integer between the current index and the last index of the array. Then, we swap the elements at the current index and the chosen index - this simulates drawing (and removing) the element from the hat, as the next range from which we select a random index will not include the most recently processed one. One small, yet important detail is that it is possible to swap an element with itself - otherwise, some array permutations would be more likely than others.

```java
class Solution {

    int[] originalNums;
    int[] currNums;
    Random random;
    public Solution(int[] nums) {
        this.originalNums = nums;
        this.currNums = nums.clone();
        this.random = new Random(System.currentTimeMillis());
    }

    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        currNums = originalNums.clone();
        return this.currNums;
    }

    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        for(int i = 0; i < currNums.length; i++) {
            int tmp = currNums[i];
            int idx = random.nextInt(currNums.length - i) + i;
            currNums[i] = currNums[idx];
            currNums[idx] = tmp;
        }
        return currNums;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int[] param_1 = obj.reset();
 * int[] param_2 = obj.shuffle();
 */
```

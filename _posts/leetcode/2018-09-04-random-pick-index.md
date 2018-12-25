---
layout: post
title: "Leetcode 398: Random Pick Index"
date: '2018-09-04 22:54'
tags:
  - Leetcode
  - ReservoirSampling
---

# Question
Given an array of integers with possible duplicates, randomly output the index of a given target number. You can assume that the given target number must exist in the array.

Note:

The array size can be very large. Solution that uses too much extra space will not pass the judge.

Example:

```
int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(3);

// pick(1) should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(1);
```

# Reservoir Sampling: $$O(1)$$ space and $$O(n)$$ time Solution
```java
public class Solution {
    int[] nums;
    Random rand;

    public Solution(int[] nums) {
        this.nums = nums;
        this.rand = new Random();
    }

    public int pick(int target) {
        int total = 0;
        int res = -1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) {
                // randomly select an int from 0 to the number of occurrences of target.
                // If x equals 0, set the res as the current index. The probability is
                // always 1/nums for the latest appeared number. For example, 1 for 1st num,
                // 1/2 for 2nd num, 1/3 for 3nd num (1/2 * 2/3 for each of the first 2 nums).
                int x = rand.nextInt(++total);
                res = x == 0 ? i : res;
            }
        }
        return res;
    }
}
```

# $$O(n)$$ space and $$O(1)$$ time Solution

```java
class Solution {

    Map<Integer, ArrayList<Integer>> idxMap;
    Random random;

    public Solution(int[] nums) {
        idxMap = new HashMap<>();
        random = new Random(System.currentTimeMillis());
        for(int i = 0; i < nums.length; i++) {
            idxMap.computeIfAbsent(nums[i], k -> new ArrayList<Integer>()).add(i);
        }
    }

    public int pick(int target) {
        ArrayList<Integer> idx = idxMap.get(target);
        return idx.get(random.nextInt(idx.size()));
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int param_1 = obj.pick(target);
 */
```

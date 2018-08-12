---
layout: "post"
title: "Leetcode 349: Intersection of Two Arrays"
date: "2018-08-09 22:53"
tags:
  - Leetcode
---

# Question
Given two arrays, write a function to compute their intersection.

Example 1:

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

Example 2:

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```

Note:

* Each element in the result must be unique.
* The result can be in any order.


# Solution
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set1 = new HashSet<>(), set2 = new HashSet<>();
        for(int n : nums1) {
            set1.add(n);
        }
        for(int n : nums2) {
            if(set1.contains(n)) {
                set2.add(n);
            }
        }
        int[] res = new int[set2.size()];
        int i = 0;
        for(int n: set2) {
            res[i++] = n;
        }
        return res;
    }
}
```

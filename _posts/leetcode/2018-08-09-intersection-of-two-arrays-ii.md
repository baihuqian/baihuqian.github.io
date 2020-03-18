---
layout: post
title: 'Leetcode 350: Intersection of Two Arrays II'
date: '2018-08-09 23:02'
tags:
  - Leetcode
published: true
---

Given two arrays, write a function to compute their intersection.

Example:
Given `nums1 = [1, 2, 2, 1]`, `nums2 = [2, 2]`, return `[2, 2]`.

Note:

* Each element in the result should appear as many times as it shows in both arrays.
* The result can be in any order.

Follow up:
* What if the given array is already sorted? How would you optimize your algorithm?
* What if nums1's size is small compared to nums2's size? Which algorithm is better?
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

# Solution
```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Integer> count1 = new HashMap<>();

        for(int n : nums1) {
            if(count1.containsKey(n)) {
                count1.put(n, count1.get(n) + 1);
            } else {
                count1.put(n, 1);
            }
        }
        ArrayList<Integer> intersection = new ArrayList<>();
        for(int n : nums2) {
            if(count1.containsKey(n)) {
                intersection.add(n);
                if(count1.get(n) > 1) {
                    count1.put(n, count1.get(n) - 1);
                } else {
                    count1.remove(n);
                }
            }
        }
        int[] res = new int[intersection.size()];
        int i = 0;
        for(int n: intersection) {
            res[i++] = n;
        }
        return res;
    }
}
```

---
layout: "post"
title: "Leetcode 347: Top K Frequent Elements"
date: "2018-08-11 20:39"
tags:
  - Leetcode
---

# Question
Given a non-empty array of integers, return the k most frequent elements.

Example 1:
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

Example 2:

```
Input: nums = [1], k = 1
Output: [1]
```

Note:

* You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
* Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

# $$O(nlogn)$$ Solution
Use a heap to find the k most frequent elements.
```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> frequency = new HashMap<>();
        for(int n: nums) {
            frequency.put(n, frequency.getOrDefault(n, 0) + 1);
        }

        PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>(Map.Entry.comparingByValue(Comparator.reverseOrder()));
        for(Map.Entry<Integer, Integer> entry: frequency.entrySet()) {
            pq.offer(entry);
        }
        List<Integer> res = new ArrayList<>(k);
        for(int i = 0; i < k; i++) {
            res.add(pq.poll().getKey());
        }
        return res;
    }
}
```

# $$O(n)$$ Solution
Use bucket sort to find frequencies in descending order.

```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> frequency = new HashMap<>();

        for(int n: nums) {
            frequency.put(n, frequency.getOrDefault(n, 0) + 1);
        }

        Map<Integer, List<Integer>> buckets = new TreeMap<>();
        for(Map.Entry<Integer, Integer> entry: frequency.entrySet()) {
            buckets.computeIfAbsent(entry.getValue(), kk -> new LinkedList<>()).add(entry.getKey());
        }
        List<Integer> res = new ArrayList<>(k);
        for(int i = 0, j = nums.length; j > 0 && i < k; j--) {
            if(buckets.containsKey(j)) {
                res.addAll(buckets.get(j));
                i += buckets.get(j).size();
            }
        }
        return res;
    }
}
```

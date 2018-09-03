---
layout: "post"
title: "Leetocde 382: Linked List Random Node"
date: "2018-08-26 17:38"
tags:
  - Leetcode
  - Review
  - ReservoirSampling
---

# Question
Given a singly linked list, return a random node's value from the linked list. Each node must have the same probability of being chosen.

Follow up:

What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?

Example:

```
// Init a singly linked list [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
solution.getRandom();
```

# Solution
Use Reservoir Sampling.

Problem:

Choose `k` entries from `n` numbers. Make sure each number is selected with the probability of `k/n`.

Basic idea:

1. Choose `1, 2, 3, ..., k` first and put them into the reservoir.
1. For `k+1`, pick it with a probability of `k/(k+1)` (this can be done via `Random.nextInt(k+1) < k`), and randomly replace a number in the reservoir.
1. For `k+i`, pick it with a probability of `k/(k+i)`, and randomly replace a number in the reservoir.
1. Repeat until `k+i` reaches `n`.

Proof:

1. For `k+i`, the probability that it is selected and will replace a number in the reservoir is `k/(k+i)`
1. For a number in the reservoir before (let's say `x`), the probability that it keeps staying in the reservoir is

```
P(x was in the reservoir last time) × P(X is not replaced by k+i)
= P(X was in the reservoir last time) × (1 - P(k+i is selected and replaces X))
= k/(k+i-1) × （1 - k/(k+i) × 1/k）
= k/(k+i)
```
1. When `k+i` reaches `n`, the probability of each number staying in the reservoir is `k/n`.

An implementation of Reservoir Sampling is as follows:

```java
int[] reservoir = new int[k];
Random random = new Random(System.currentTimeMillis());
for(int i = 0; i < k; i++) reservoir[i] = input[i];
for(int j = k; k < n; k++) {
    int rnd = random.nextInt(j);
    if(rnd < k)
        reservoir[rnd] = input[j];
}
return reservoir;
```

The solution of this question is as follows:

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    ListNode head;
    Random random;
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    public Solution(ListNode head) {
        this.head = head;
        this.random = new Random(System.currentTimeMillis());
    }

    /** Returns a random node's value. */
    public int getRandom() {
        if(head == null) return -1;
        ListNode res = head, node = head;
        int i = 1;
        while(node.next != null) {
            node = node.next;
            if(random.nextInt(++i) == 0)
                res = node;
        }
        return res.val;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(head);
 * int param_1 = obj.getRandom();
 */
```

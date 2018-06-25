---
layout: "post"
title: "Leetcode 137: Single Number II"
date: "2018-06-23 23:16"
tags:
  - Leetcode
  - StateMachine
---

# Question
Given a non-empty array of integers, every element appears three times except for one, which appears exactly once. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

Example 1:
```
Input: [2,2,3,2]
Output: 3
```

Example 2:
```
Input: [0,1,0,1,0,1,99]
Output: 99
```

# Solution
To determine how many occurrences of an integer there are up to a maximum of three. We need to build a 3 state counter.

How do we do that? Boolean logic. Consider a single bit input and how that would drive the internal state of a bit counter in the state table below. An input value '0' does not change the internal state. Input of '1' drives the counter from state 0, 1, 2 and resets back to zero.

```
/* 3-state counter */
'A'  'twos'  'ones'    'twos'  'ones'
 0      0       0    |    0       0
 0      0       1    |    0       1
 0      1       0    |    1       0
 1      0       0    |    0       1
 1      0       1    |    1       0
 1      1       0    |    0       0
```

How to derive the state transfer from the above state table? Karnaugh map.

`ones = A XOR ones if twos == 0 else 0`, and `twos = twos XOR A if new_ones == 0 else 0`, in which `new_ones` is the output from statement 1.

```
  for (int i = 0; i < A.size(); i++) {
    ones = (ones ^ a[i]) & ~twos;
    twos = (twos ^ a[i]) & ~ones;
  }
```

We can extend the same approach to a 4-state counter, which works on the same problem if we want to find a single occurrence among multiple occurrences of 4 values. The state table extends by one count before resetting, and we have a new set of boolean logic. Again, other boolean constructions are possible for the same state table.

```
/* 4-state counter */
'A'  'twos'  'ones'    'twos'  'ones'
 0      0       0    |    0       0
 0      0       1    |    0       1
 0      1       0    |    1       0
 0      1       1    |    1       1
 1      0       0    |    0       1
 1      0       1    |    1       0
 1      1       0    |    1       1
 1      1       1    |    0       0

  for (int i = 0; i < A.size(); i++) {
    ones = (ones ^ A[i]);
    twos = (twos | A[i]) & (~A[i] | (ones ^ twos));
  }

```

For 5 states we would need to add a third bit as a 'threes' variable and additional logic design.

```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        ones, twos = 0, 0
        for n in nums:
            ones = (ones ^ n) & ~twos
            twos = (twos ^ n) & ~ones

        return ones
```

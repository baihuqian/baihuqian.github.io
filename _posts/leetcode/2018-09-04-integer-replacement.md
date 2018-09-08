---
layout: "post"
title: "Leetcode 397: Integer Replacement"
date: "2018-09-04 22:45"
tags:
  - Leetcode
---

# Question
Given a positive integer n and you can do operations as follow:

1. If `n` is even, replace `n` with `n/2`.
1. If `n` is odd, you can replace n with either `n + 1` or `n - 1`.

What is the minimum number of replacements needed for n to become 1?

Example 1:

```
Input:
8

Output:
3

Explanation:
8 -> 4 -> 2 -> 1
```


Example 2:
```
Input:
7

Output:
4

Explanation:
7 -> 8 -> 4 -> 2 -> 1
or
7 -> 6 -> 3 -> 2 -> 1
```

# Solution
Be careful about `n + 1` because it can overflow.
```java
class Solution {
    public int integerReplacement(int n) {
        if(n == 1) return 0;
        else {
            if((n & 1) == 0) return 1 + integerReplacement(n >> 1);
            else return 2 + Math.min(integerReplacement((n >> 1) + 1), integerReplacement(n >> 1));
        }
    }
}
```

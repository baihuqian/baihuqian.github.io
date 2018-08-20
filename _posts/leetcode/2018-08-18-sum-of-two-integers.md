---
layout: "post"
title: "Leetcode 371: Sum of Two Integers"
date: "2018-08-18 11:33"
---

# Question
Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

Example:
Given a = 1 and b = 2, return 3.

# Solution
Compute sum (using xor) and carry (using and) separately.

```java
class Solution {
    public int getSum(int a, int b) {
        if(b == 0) return a;
        int sum = a ^ b; // sum for each bit, ignoring carry
        int carry = (a & b) << 1;
        return getSum(sum, carry);
    }
}
```

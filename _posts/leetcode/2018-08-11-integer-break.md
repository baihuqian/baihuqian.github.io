---
layout: "post"
title: "Leetcode 343: Integer Break"
date: "2018-08-11 20:06"
tags:
  - Leetcode
---

# Question
Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.

Example 1:

```
Input: 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.
```

Example 2:

```
Input: 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
```

Note: You may assume that n is not less than 2 and not larger than 58.

# Solution
```java
class Solution {
    public int integerBreak(int n) {
        int product = 0, breaks = 2;
        while(breaks <= n) {
            int base = n / breaks, residue = n % breaks;
            int newProduct = 1;
            for(int i = breaks; i > 0; i--) {
                if(residue > 0) {
                    newProduct *= base + 1;
                    residue--;
                } else newProduct *= base;
            }
            if(newProduct > product)
                product = newProduct;
            else break;
            breaks++;
        }
        return product;
    }
}
```

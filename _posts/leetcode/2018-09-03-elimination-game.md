---
layout: "post"
title: "Leetcode 390: Elimination Game"
date: "2018-09-03 13:46"
tags:
  - Leetcode
---

# Question
There is a list of sorted integers from 1 to n. Starting from left to right, remove the first number and every other number afterward until you reach the end of the list.

Repeat the previous step again, but this time from right to left, remove the right most number and every other number from the remaining numbers.

We keep repeating the steps again, alternating left to right and right to left, until a single number remains.

Find the last number that remains starting with a list of length n.

Example:

```
Input:
n = 9,
1 2 3 4 5 6 7 8 9
2 4 6 8
2 6
6

Output:
6
```

# Solution
Every run, it reduces the search space by half. We keep track of the first and last number. If odd number of numbers are eliminated, we adjust both first and last number; otherwise, only the "first" number in each direction is adjusted. We loop until first and last number are the same.

```java
class Solution {
    public int lastRemaining(int n) {
        if(n == 1) return 1;

        int first = 1, last = n;
        int count = n;
        for(int step = 1, digit = 1; first != last; step <<= 1, digit++) {
            if(digit % 2 == 1) {
                first += step;
                if(count % 2 == 1)
                    last -= step;
            } else {
                last -= step;
                if(count % 2 == 1)
                    first += step;
            }
            count /= 2;
        }
        return first;
    }
}
```

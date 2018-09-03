---
layout: "post"
title: "Leetcode 386: Lexicographical Numbers"
date: "2018-09-01 13:48"
tags:
  - Leetcode
---

# Question
Given an integer n, return 1 - n in lexicographical order.

For example, given 13, return: `[1,10,11,12,13,2,3,4,5,6,7,8,9]`.

Please optimize your algorithm to use less time and space. The input size may be as large as 5,000,000.

# Solution
```java
class Solution {
    public List<Integer> lexicalOrder(int n) {
        int last = 1;
        int[] stack = new int[len(n)];
        int digits = 1;
        stack[0] = 1;

        List<Integer> res = new ArrayList<>(n);
        res.add(last);
        for(int i = 2; i <= n; i++) {
            if(last * 10 <= n) {
                stack[digits++] = 0;
                last *= 10;
            }
            else {
                int lastDigit = stack[--digits];
                while(lastDigit == 9 || last + 1 > n) {
                    lastDigit = stack[--digits];
                    last /= 10;
                }
                stack[digits++] = lastDigit + 1;
                last++;
            }
            res.add(last);
        }
        return res;
    }

    private int len(int n) {
        int res = 0;
        while (n > 0) {
            n /= 10;
            res++;
        }
        return res;
    }
}
```

---
layout: post
title: 'Leetcode 346: Moving Average from Data Stream'
date: '2018-08-09 22:41'
tags:
  - Leetcode
published: true
---

# Question
Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

Example:

```
MovingAverage m = new MovingAverage(3);
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3
```

# Solution
```java
class MovingAverage {
    int [] num;
    int pos;
    int sum;
    int count;

    /** Initialize your data structure here. */
    public MovingAverage(int size) {
        num = new int[size];
        pos = 0;
        sum = 0;
        count = 0;
    }

    public double next(int val) {
        sum -= num[pos];
        num[pos++] = val;
        if(pos == num.length) {
            pos = 0;
        }
        sum += val;
        count = count == num.length ? count : count + 1;
        return ((double) sum) / count;
    }
}

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param_1 = obj.next(val);
 */
```

---
layout: "post"
title: "Leetcode 385: Mini Parser"
date: "2018-08-27 21:12"
tags:
  - Leetcode
---

# Question
Given a nested list of integers represented as a string, implement a parser to deserialize it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Note: You may assume that the string is well-formed:

* String is non-empty.
* String does not contain white spaces.
* String contains only digits 0-9, [, - ,, ].

Example 1:
```
Given s = "324",

You should return a NestedInteger object which contains a single integer 324.
```

Example 2:
```
Given s = "[123,[456,[789]]]",

Return a NestedInteger object containing a nested list with 2 elements:

1. An integer containing value 123.
2. A nested list containing two elements:
    i.  An integer containing value 456.
    ii. A nested list with one element:
         a. An integer containing value 789.

```

# Solution
If the string starts with `'['`, then it must be a NestedInteger with a list; otherwise it will be a nested integer with a single item.
```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
class Solution {
    public NestedInteger deserialize(String s) {
        Stack<NestedInteger> stack = new Stack<>();
        boolean inNum = false;
        int sign = 1;
        int curr = 0;
        NestedInteger currInt = null;
        for(char c: s.toCharArray()) {
            if(c >= '0' && c <= '9') {
                inNum = true;
                curr = curr * 10 + (c - '0');
            } else if (c == ',') {
                if(inNum) {
                    if(currInt == null)
                        currInt = new NestedInteger(curr * sign);
                    else
                        currInt.add(new NestedInteger(curr * sign));
                    curr = 0;
                    sign = 1;
                    inNum = false;
                }
            } else if (c == '[') {
                if(currInt != null) {
                    stack.push(currInt);
                }
                currInt = new NestedInteger();
            } else if (c == ']') {
                if(inNum) {
                    currInt.add(new NestedInteger(curr * sign));
                    curr = 0;
                    inNum = false;
                    sign = 1;
                }
                if(stack.size() > 0) {
                    NestedInteger last = stack.pop();
                    last.add(currInt);
                    currInt = last;
                }
            } else if (c == '-') {
                sign = -1;
            }
        }
        if(currInt == null && inNum)
            currInt = new NestedInteger(curr * sign);
        if(currInt == null)
            currInt = new NestedInteger();

        return currInt;
    }
}
```

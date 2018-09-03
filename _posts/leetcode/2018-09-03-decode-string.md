---
layout: "post"
title: "Leetcode 394: Decode String"
date: "2018-09-03 15:08"
tags:
  - Leetcode
---

# Question
Given an encoded string, return it's decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

Examples:

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```


# Solution
class Solution {
    public String decodeString(String s) {
        if (s.length() == 0) return s;
        StringBuilder res = new StringBuilder();
        StringBuilder partial = res;
        Stack<StringBuilder> sbStack = new Stack<>();
        Stack<Integer> timeStack = new Stack<>();
        int times = 0;

        for(char c: s.toCharArray()) {
            if(c >= '0' && c <= '9') {
                times = times * 10 + (c - '0');
            } else if (c == '[') {
                sbStack.push(partial);
                timeStack.push(times);
                partial = new StringBuilder();
                times = 0;
            } else if (c == ']') {
                String segment = partial.toString();
                partial = sbStack.pop();
                times = timeStack.pop();
                for(int i = 0; i < times; i++)
                    partial.append(segment);
                times = 0;
            } else {
                partial.append(c);
            }
        }
        return res.toString();
    }
}
```java
```

---
layout: "post"
title: "Leetcode 395: Longest Substring with At Least K Repeating Characters"
date: "2018-09-03 16:23"
tags:
  - Leetcode
---

# Question
Find the length of the longest substring T of a given string (consists of lowercase letters only) such that every character in T appears no less than k times.

Example 1:
```
Input:
s = "aaabb", k = 3

Output:
3

The longest substring is "aaa", as 'a' is repeated 3 times.
```

Example 2:

```
Input:
s = "ababbc", k = 2

Output:
5

The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

# Solution
In each step, just find the infrequent elements (show less than k times) as splits since any of these infrequent elements couldn't be any part of the substring we want.

```java
public class Solution {
    public int longestSubstring(String s, int k) {
        if (s == null || s.length() == 0) return 0;
        char[] chars = new char[26];
        // record the frequency of each character
        for (int i = 0; i < s.length(); i++)
          chars[s.charAt(i) - 'a'] += 1;

        boolean flag = true;
        for (int i = 0; i < chars.length; i++) {
            if (chars[i] < k && chars[i] > 0) {
                flag = false;
                break;
            }
        }
        // return the length of string if this string is a valid string
        if (flag == true) return s.length();
        int result = 0;
        int start = 0, cur = 0;
        // otherwise we use all the infrequent elements as splits
        while (cur < s.length()) {
            if (chars[s.charAt(cur) - 'a'] < k) {
                result = Math.max(result, longestSubstring(s.substring(start, cur), k));
                start = cur + 1;
            }
            cur++;
        }
        // last segment
        result = Math.max(result, longestSubstring(s.substring(start), k));
        return result;
    }
}
```

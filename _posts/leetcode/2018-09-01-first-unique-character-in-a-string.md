---
layout: "post"
title: "Leetcode 387: First Unique Character in a String"
date: "2018-09-01 13:55"
tags:
  - Leetcode
---

# Question
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

Examples:
```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```

Note: You may assume the string contain only lowercase letters.

# Solution
```java
class Solution {
    public int firstUniqChar(String s) {
        int[] charMap = new int[26];
        Arrays.fill(charMap, -1);

        for(int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if(charMap[c - 'a'] == -1) // -1 means the char does not exist in array yet
                charMap[c - 'a'] = i;
            else if (charMap[c - 'a'] >= 0)
                charMap[c - 'a'] = -2; // -2 means the char occurs more than once in the array
        }
        int minIndex = s.length();
        for(int idx : charMap) {
            if (idx >= 0)
                minIndex = Math.min(minIndex, idx);
        }
        return minIndex == s.length() ? -1 : minIndex;
    }
}
```

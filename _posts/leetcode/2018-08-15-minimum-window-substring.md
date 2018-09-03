---
layout: "post"
title: "Leetcode 76: Minimum Window Substring"
date: "2018-08-15 21:52"
tags:
  - Leetcode
  - Review
  - SlidingWindow
  - Hard
---

# Question
Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

Example:
```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

Note:

* If there is no such window in S that covers all characters in T, return the empty string `""`.
* If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

# Solution
Use a sliding window with two pointers, assisted with a Hash Map. If the string is ascii-encoded, we can replace the hashmap with an array of size 128.
```java
class Solution {
    public String minWindow(String s, String t) {
        if(s.length() == 0 || t.length() == 0)
            return "";

        int[] count = new int[128];
        Arrays.fill(count ,0);
        for(char c: t.toCharArray()) {
            count[c]++;
        }
        int counter = t.length(), l = 0, r = 0, len = Integer.MAX_VALUE, head = 0;
        while(r < s.length()) {
            if(count[s.charAt(r++)]-- > 0) { // decrement the remaining count for character at index r
                counter--;
            }
            while(counter == 0) {
                if(r - l < len) {
                    len = r - l;
                    head = l;
                }
                // increment the remaining count for character at index l
                if(++count[s.charAt(l++)] > 0) counter++;
            }
        }
        return len == Integer.MAX_VALUE ? "" : s.substring(head, head + len);
    }
}
```

For most substring problem, we are given a string and need to find a substring of it which satisfy some restrictions. A general way is to use a hash map assisted with two pointers. The template is given below. The idea is to extend the window to the right until the restrictions are satisfied. Then shrink the window from the left until the restrictions are not satisfied.

The correctness of this solution can be proven by contradiction.

```java
public int findSubstring(String s) {
    int[] map = new int[128];
    Arrays.fill(map, 0);
    int counter; // check whether the substring is valid
    int begin = 0, end = 0; //two pointers, one point to tail and one  head
    int d; //the length of substring

    for() { /* initialize the hash map here */ }

    while(end < s.length()){

        if(map[s[end++]]-- ?){  /* modify counter here */ }

        while(/* counter condition that matches the substring */){

             /* update d here if finding minimum*/

            //increase begin to make it invalid/valid again
          if(++map[s[begin++]] ?){ /*modify counter here*/ }
        }  

        /* update d here if finding maximum*/
    }
    return d;
  }
```

One thing needs to be mentioned is that when asked to find maximum substring, we should update maximum after the inner while loop to guarantee that the substring is valid. On the other hand, when asked to find minimum substring, we should update minimum inside the inner while loop.

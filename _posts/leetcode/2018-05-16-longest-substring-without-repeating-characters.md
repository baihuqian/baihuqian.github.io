---
layout: "post"
title: "Leetcode 3: Longest Substring Without Repeating Characters"
date: "2018-05-16 22:17"
tags: Leetcode
---

# Question
Given a string, find the length of the longest substring without repeating characters.

Examples:

Given "`abcabcbb`", the answer is "`abc`", which the length is 3.

Given "`bbbbb`", the answer is "`b`", with the length of 1.

Given "`pwwkew`", the answer is "`wke`", with the length of 3. Note that the answer must be a substring, "`pwke`" is a subsequence and not a substring.

# Solution

In the naive approaches, we repeatedly check a substring to see if it has duplicate character. But it is unnecessary. If a substring $$s_{ij}$$ from index $$i$$ to $$j - 1$$ is already checked to have no duplicate characters. We only need to check if `s[j]` is already in the substring $$s_{ij}$$.

To check if a character is already in the substring, we can scan the substring, which leads to an $$O(n^2)$$ algorithm. But we can do better.

By using Set as a sliding window, checking if a character in the current can be done in $$O(1)$$.

A sliding window is an abstract concept commonly used in array/string problems. A window is a range of elements in the array/string which usually defined by the start and end indices, i.e. $$[i, j)$$. A sliding window is a window "slides" its two boundaries to the certain direction. For example, if we slide $$[i, j)$$ to the right by $$1$$ element, then it becomes $$[i+1, j+1)$$.

Back to our problem. We use HashSet to store the characters in current window $$[i, j)$$ ($$j = i + 1$$ initially). Then we slide the index $$j$$ to the right. If it is not in the HashSet, we slide $$j$$ further. Doing so until `s[j]` is already in the HashSet. At this point, we found the maximum size of substrings without duplicate characters start with index $$i$$. Next, to include $$j$$ in the substring, we need to slide $$i$$ further until `s[j]` is not in the set, or when $$i == j$$ (when `s[j]` is the same as `s[j-1]`). In the latter case, we slide both $$i$$ and $$j$$ further and repeat. We maintain $$ 0 \leq i \lt j \lt len(s)$$ relationship.

```python
class Solution:
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s) <= 1:
            return len(s)

        charSet = set(s[0])
        ans = 1
        i = 0
        j = 1
        while i < j < len(s):
            if s[j] not in charSet:
                charSet.add(s[j])
                j += 1
                if len(charSet) > ans:
                    ans = j - i
            else:
                charSet.remove(s[i])
                i += 1
                if i == j and i < len(s):
                    j += 1
                    charSet.add(s[i])

        return ans
```

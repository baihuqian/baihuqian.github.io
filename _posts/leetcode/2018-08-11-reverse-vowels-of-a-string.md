---
layout: "post"
title: "Leetcode 345: Reverse Vowels of a String"
date: "2018-08-11 20:15"
tags:
  - Leetcode
---

# Question
Write a function that takes a string as input and reverse only the vowels of a string.

Example 1:
```
Input: "hello"
Output: "holle"
```

Example 2:

```
Input: "leetcode"
Output: "leotcede"
```

Note:

The vowels does not include the letter "y".

# Solution
```java
class Solution {
    private static final Set<Character> VOWEL = new HashSet<>(Arrays.asList('a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'));

    public String reverseVowels(String s) {
        int left = 0, right = s.length() - 1;
        char[] charArray = s.toCharArray();
        while(left < right) {
            if(!VOWEL.contains(charArray[left])) left++;
            else if(!VOWEL.contains(charArray[right])) right--;
            else {
                char tmp = charArray[left];
                charArray[left] = charArray[right];
                charArray[right] = tmp;
                left++;
                right--;
            }
        }
        return new String(charArray);
    }
}
```

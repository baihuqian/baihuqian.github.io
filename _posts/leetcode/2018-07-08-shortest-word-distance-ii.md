---
layout: "post"
title: "Leetcode 244: Shortest Word Distance II"
date: "2018-07-08 14:00"
tags:
  - Leetcode
---

# Question
Design a class which receives a list of words in the constructor, and implements a method that takes two words word1 and word2 and return the shortest distance between these two words in the list. Your method will be called repeatedly many times with different parameters.

Example:

Assume that `words = ["practice", "makes", "perfect", "coding", "makes"]`.

```
Input: word1 = “coding”, word2 = “practice”
Output: 3
Input: word1 = "makes", word2 = "coding"
Output: 1
```

Note:

You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.

# Solution
```python
class WordDistance(object):

    def __init__(self, words):
        """
        :type words: List[str]
        """
        self.word_dict = dict()
        self.length = len(words)
        for i, w in enumerate(words):
            if w in self.word_dict:
                self.word_dict[w].append(i)
            else:
                self.word_dict[w] = [i]


    def shortest(self, word1, word2):
        """
        :type word1: str
        :type word2: str
        :rtype: int
        """
        i1, i2 = 0, 0
        d1, d2 = self.word_dict[word1], self.word_dict[word2]
        min_d = self.length
        while True:
            p1, p2 = d1[i1], d2[i2]
            min_d = min(min_d, abs(p1 - p2))
            if p1 < p2:
                i1 += 1
            else:
                i2 += 1

            if i1 >= len(d1) or i2 >= len(d2):
                break

        return min_d




# Your WordDistance object will be instantiated and called as such:
# obj = WordDistance(words)
# param_1 = obj.shortest(word1,word2)
```

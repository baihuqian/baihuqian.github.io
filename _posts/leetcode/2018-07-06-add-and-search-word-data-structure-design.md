---
layout: "post"
title: "Leetcode 211: Add and Search Word - Data structure design"
date: "2018-07-06 21:31"
tags:
  - Leetcode
---

# Question
Design a data structure that supports the following two operations:

```
void addWord(word)
bool search(word)
search(word) can search a literal word or a regular expression string containing only letters a-z or .. A . means it can represent any one letter.
```

Example:

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

Note:

You may assume that all words are consist of lowercase letters a-z.

# Solution
```python
class TrieNode(object):
    def __init__(self):
        self.isExists = False
        self.next = dict()

class WordDictionary(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = TrieNode()

    def addWord(self, word):
        """
        Adds a word into the data structure.
        :type word: str
        :rtype: void
        """
        node = self.root
        for c in word:
            if c not in node.next:
                node.next[c] = TrieNode()
            node = node.next[c]
        node.isExists = True


    def search(self, word):
        """
        Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
        :type word: str
        :rtype: bool
        """
        search_queue = list()
        search_queue.append((word, self.root))
        while len(search_queue) > 0:
            w, node = search_queue.pop()
            if len(w) == 0:
                if node.isExists:
                    return True
            else:
                c, new_w = w[0], w[1:]
                if c != '.':
                    if c in node.next:
                        search_queue.append((new_w, node.next[c]))
                else:
                    for ch in node.next:
                        search_queue.append((new_w, node.next[ch]))
        return False


# Your WordDictionary object will be instantiated and called as such:
# obj = WordDictionary()
# obj.addWord(word)
# param_2 = obj.search(word)
```

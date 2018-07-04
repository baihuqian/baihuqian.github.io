---
layout: "post"
title: "Leetcode 208: Implement Trie (Prefix Tree)"
date: "2018-07-04 21:18"
tags:
  - Leetcode
---

# Question
Implement a trie with `insert`, `search`, and `startsWith` methods.

Example:
```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

Note:

* You may assume that all inputs are consist of lowercase letters a-z.
* All inputs are guaranteed to be non-empty strings.

# Solution
```python
class Node(object):
    def __init__(self):
        self.isEnd = False
        self.next = [None] * 26

class Trie(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = Node()


    def insert(self, word):
        """
        Inserts a word into the trie.
        :type word: str
        :rtype: void
        """
        node = self.root
        for c in word:
            idx = self._index(c)
            if node.next[idx] is None:
                node.next[idx] = Node()
            node = node.next[idx]
        node.isEnd = True


    def search(self, word):
        """
        Returns if the word is in the trie.
        :type word: str
        :rtype: bool
        """
        node = self.root
        for c in word:
            idx = self._index(c)
            if node.next[idx] is None:
                return False
            node = node.next[idx]
        return node.isEnd


    def startsWith(self, prefix):
        """
        Returns if there is any word in the trie that starts with the given prefix.
        :type prefix: str
        :rtype: bool
        """
        node = self.root
        for c in prefix:
            idx = self._index(c)
            if node.next[idx] is None:
                return False
            node = node.next[idx]
        return True

    def _index(self, c):
        return ord(c) - ord('a')


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

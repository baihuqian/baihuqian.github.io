---
layout: "post"
title: "Leetcode 68: Text Justification"
date: "2018-07-29 14:39"
tags:
  - Leetcode
  - Hard
---

# Question
Given an array of words and a width maxWidth, format the text such that each line has exactly maxWidth characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '` when necessary so that each line has exactly maxWidth characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left justified and no extra space is inserted between words.

Note:

* A word is defined as a character sequence consisting of non-space characters only.
* Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.
* The input array words contains at least one word.

Example 1:

```
Input:
words = ["This", "is", "an", "example", "of", "text", "justification."]
maxWidth = 16
Output:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

Example 2:

```
Input:
words = ["What","must","be","acknowledgment","shall","be"]
maxWidth = 16
Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
Explanation: Note that the last line is "shall be    " instead of "shall     be",
             because the last line must be left-justified instead of fully-justified.
             Note that the second line is also left-justified becase it contains only one word.
```

Example 3:

```
Input:
words = ["Science","is","what","we","understand","well","enough","to","explain",
         "to","a","computer.","Art","is","everything","else","we","do"]
maxWidth = 20
Output:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

# Solution
```python
class Solution(object):
    def fullJustify(self, words, maxWidth):
        """
        :type words: List[str]
        :type maxWidth: int
        :rtype: List[str]
        """
        res = list()
        currLine = list()
        wordCount = 0
        lineLen = 0
        for w in words:
            newLineLen = lineLen
            if newLineLen == 0:
                newLineLen += len(w)
            else:
                newLineLen += len(w) + 1 # space
            # If the next word cannot fit into currLine, build the line and add the current word to next line
            if newLineLen > maxWidth:
                res.append(self.buildLine(currLine, wordCount, lineLen, maxWidth))
                currLine = [w]
                wordCount = 1
                lineLen = len(w)
            else:
                currLine.append(w)
                wordCount += 1
                lineLen = newLineLen

        if wordCount > 0:
            line = ' '.join(currLine)
            line += ' ' * (maxWidth - lineLen)
            res.append(line)
        return res

    def buildLine(self, words, wordCount, lineLen, maxWidth):
        extraSpaces = maxWidth - lineLen
        if wordCount == 1:
            return words[0] + ' ' * extraSpaces
        else:
            avgSpaces = extraSpaces // (wordCount - 1) + 1
            additionalSpaces = extraSpaces % (wordCount - 1)
            lineBuilder = list()
            for i in range(wordCount - 1):
                lineBuilder.append(words[i])
                lineBuilder.append(' ' * avgSpaces)
                if additionalSpaces > 0:
                    lineBuilder.append(' ')
                    additionalSpaces -= 1
            lineBuilder.append(words[-1])
            return ''.join(lineBuilder)
```

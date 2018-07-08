---
layout: "post"
title: "Leetcode 232: Implement Queue using Stacks"
date: "2018-07-06 23:02"
tags:
  - Leetcode
---

# Question
Implement the following operations of a queue using stacks.

* `push(x)` -- Push element x to the back of queue.
* `pop()` -- Removes the element from in front of queue.
* `peek()` -- Get the front element.
* `empty()` -- Return whether the queue is empty.

Example:

```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```

Notes:

* You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
* Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
* You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

# Solution
```python
class MyQueue(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.instack = list()
        self.outstack = list()


    def push(self, x):
        """
        Push element x to the back of queue.
        :type x: int
        :rtype: void
        """
        self.instack.append(x)


    def pop(self):
        """
        Removes the element from in front of queue and returns that element.
        :rtype: int
        """
        if len(self.outstack) == 0:
            self.__pour()
        return self.outstack.pop()


    def peek(self):
        """
        Get the front element.
        :rtype: int
        """
        if len(self.outstack) == 0:
            self.__pour()
        return self.outstack[-1]

    def empty(self):
        """
        Returns whether the queue is empty.
        :rtype: bool
        """
        return len(self.instack) == 0 and len(self.outstack) == 0

    def __pour(self):
        while len(self.instack) > 0:
            self.outstack.append(self.instack.pop())

# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

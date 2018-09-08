---
layout: "post"
title: "Java Cheet Sheet"
date: "2018-08-10 22:24"
tags: Java
---

# Data Structure
### List
* Append: `add(E e)`. Insert into specific index: `add(int index, E e)`. Note insertion will increase the size of the list.
* Access: `get(int index)`.
* Set value: `set(int index, E e)`.


### Queue
Java provides a `Queue` interface. `LinkedList` implements it. It offers the following APIs:
* Insert into queue: `add(E e)` or `offer(E e)`.
* Peek the head of the queue, but do not remove: `element()` or `peek()` (returns `null` if the queue is empty).
* Remove and return the head of the queue: `remove()` or `poll()` (returns `null` if the queue is empty).

### Stack
Java provides a `Stack` class. It offers the followiong APIs:
* Test whether a stack is empty: `empty()`.
* Push into stack: `push(E e)`.
* Peek the top of the stack without removing it: `peek()`.
* Pop the top of the stack: `pop()`.

### Arrays
Initialize an array with non-default values: `Arrays.fill(array, value)`.

Binary Search: `Arrays.binarySearch(array, [fromIndex, toIndex,] key)`.

### Map
To initialize the value for a key if not presented in the map, use the following:

```java
map.computeIfAbsent(key, k -> new Value(f(k)));
```

# `null`-Safe Object Methods
`Objects` provides null-safe or null-tolerant static methods for computing the hash code of an object, returning a string for an object, and comparing two objects.

For example, `String.equals()` throws NPE on null string, but `Objects.equals()` returns false.

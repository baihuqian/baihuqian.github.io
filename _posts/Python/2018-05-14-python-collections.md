---
layout: "post"
title: "Python Notes: Collections"
date: "2018-05-14 22:26"
tags: Python
---

{:toc}

# Deque

Double-ended queue is a generalized data structure of a queue, for which elements can be added to or removed from either the front (head) or back (tail). Python's `collections` module implements a `deque`. Deque is preferred over list in the cases where we need quicker append and pop operations from both the ends of container, as deque provides an O(1) time complexity for append and pop operations as compared to list which provides O(n) time complexity.

Using `deque(maxlen=N)` creates a fixed-sized queue. When new items are added and the queue is full, the oldest item is automatically removed. If `maxlen` is not specified or is `None`, deques may grow to an arbitrary length.

1. `append()` :- This function is used to insert the value in its argument to the right end of deque.
2. `appendleft()` :- This function is used to insert the value in its argument to the left end of deque.
3. `pop()` :- This function is used to delete an argument from the right end of deque.
4. `popleft()` :- This function is used to delete an argument from the left end of deque.
5. `index(ele, beg, end)` :- This function returns the first index of the value mentioned in arguments, starting searching from `beg` till `end` index.
6. `insert(i, a)` :- This function inserts the value mentioned in arguments(a) at index(i) specified in arguments.
7. `remove()` :- This function removes the first occurrence of value mentioned in arguments.
8. `count()` :- This function counts the number of occurrences of value mentioned in arguments.
9. `extend(iterable)` :- This function is used to add multiple values at the right end of deque. The argument passed is an iterable.
10. `extendleft(iterable)` :- This function is used to add multiple values at the left end of deque. The argument passed is an iterable. Order is reversed as a result of left appends.
11. `reverse()` :- This function is used to reverse order of deque elements.
12. `rotate()` :- This function rotates the deque by the number specified in arguments. If the number specified is negative, rotation occurs to left. Else rotation is to right.

# Heap
## `PriorityQueue`
Python's `queue` module defines a `PriorityQueue` class.

## `heapq` Module
Heaps are binary trees for which every parent node has a value less than or equal to any of its children. This is also called min heap. This implementation uses arrays for which `heap[k] <= heap[2*k+1]` and `heap[k] <= heap[2*k+2]` for all `k`, counting elements from zero. For the sake of comparison, non-existing elements are considered to be infinite. The interesting property of a heap is that its smallest element is always the root, `heap[0]`.

Python's `heapq` provides an implementation of the heap queue algorithm, which provides min heap operations on top of a list.

1. `heapify(list)` :- This function is used to convert the list into a heap data structure. i.e. in heap order, in place.
2. `heappush(heap, ele)` :- This function is used to insert the element mentioned in its arguments into heap. The order is adjusted, so as heap structure is maintained.
3. `heappop(heap)` :- This function is used to remove and return the smallest element from heap. The order is adjusted, so as heap structure is maintained.
4. heappushpop(heap, ele) :- Push item on the heap, then pop and return the smallest item from the heap. The combined action runs more efficiently than `heappush()` followed by a separate call to `heappop()`.
5. `heapreplace(heap, ele)` :- Pop and return the smallest item from the heap, and also push the new item.
6. `nlargest(k, iterable, key = fun)` :- This function is used to return the k largest elements from the iterable specified and satisfying the key if mentioned.
7. `nsmallest(k, iterable, key = fun)` :- This function is used to return the k smallest elements from the iterable specified and satisfying the key if mentioned.
8. `merge(*iterables, key=None, reverse=False)` :- Merge multiple sorted inputs into a single sorted output, returns an iterator over the sorted values. Similar to `sorted(itertools.chain(*iterables))` but returns an iterable, does not pull the data into memory all at once, and assumes that each of the input streams is already sorted (smallest to largest).

# Counter
The `Counter` class in the `collections` module counts number of occurrences of each item. As input, Counter objects can be fed any sequence of hashable input items. It produces a dictionary that maps the items to the number of occurrences.

1. `Counter(iter)` :- Produces the counter dict.
2. `update(iter)` :- Adds the counter from the input iter to the Counter.
3. `subtract(iter)` :- Subtracts the counter from the input iter to the Counter.
3. `most_common(n)` :- Return a list of the `n` most common elements and their counts from the most common to the least. If `n` is omitted or `None`, `most_common()` returns all elements in the counter.

Other common usages:
```python
sum(c.values())                 # total of all counts
c.clear()                       # reset all counts
list(c)                         # list unique elements
set(c)                          # convert to a set
dict(c)                         # convert to a regular dictionary
c.items()                       # convert to a list of (elem, cnt) pairs
Counter(dict(list_of_pairs))    # convert from a list of (elem, cnt) pairs
c.most_common()[:-n-1:-1]       # n least common elements
+c                              # remove zero and negative counts
c + d                           # add two counters together:  c[x] + d[x]
c - d                           # subtract (keeping only positive counts)
c & d                           # intersection:  min(c[x], d[x])
c | d                           # union:  max(c[x], d[x])
```

# Custom Containers
To implement custom containers that share some features with built-in container type, such as lists and dictionaries, we can extend abstract base classes defined in the `collections.abc` module, which defines which special methods we should implement. For example, `collections.abc.Iterable` requires the implementation of the `__iter__()` method.

Many of the abstract base classes in `collections.abc` also provide default implementations of common container methods.

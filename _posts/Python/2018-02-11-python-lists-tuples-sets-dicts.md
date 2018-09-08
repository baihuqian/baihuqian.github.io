---
layout: "post"
title: "Python Notes: Lists, Tuples, Sets, and Dicts"
date: "2018-02-11 10:54"
tags: Python
---
{:toc}

# Lists
Much like `ArrayList`, but it can hold multiple datatypes.

Syntax: `a_list = ['a', 1]`, square brackets with comma-separated list of values. To create a one-element list, use `[ val ]`. `list(val)` converts any iterable to a list, but it will fail if `val` is not iterable.

An empty list is evaluated to `False`, all non-empty lists are `True`.

## How are lists implemented in CPython?
CPython’s lists are really variable-length arrays, not Lisp-style linked lists. The implementation uses a contiguous array of references to other objects, and keeps a pointer to this array and the array’s length in a list head structure.

This makes indexing a list a[i] an operation whose cost is independent of the size of the list or the value of the index.

When items are appended or inserted, the array of references is resized. Some cleverness is applied to improve the performance of appending items repeatedly; when the array must be grown, some extra space is allocated so the next few times don’t require an actual resize.

## List Operations
* Index are zero-based.
* Negative index is counting backwards from the end. Much like `a_list[-n] == a_list[len(a_list)-n]`.
* `a_list[begin:end]` is slicing. It returns a new list from `a_list[begin]` up but not including `a_list[end]`. If `begin == 0` and/or `end == len(a_list)`, they can be omitted, so `a_list[:]` returns a copy of `a_list`. Indices in slicing can also be negative.
* `+` can be used to concatenate lists.
* `a_list.append(var)` appends `var` to `a_list`'s end. It takes arbitrary types of variables and consider it as one. It returns `NoneType`.
* `a_list.extend(another_list)` concatenates `another_list` to the end of `a_list`.
* `a_list.insert(idx, var)` inserts `var` at `idx`th positions. Existing elements shifts right.
*  `a_list.count(var)` counts the number of occurrences of `var` in `a_list`. If `var` is not present, it returns 0.
*  `var in a_list` returns `True` if `a_list` contains `var`. It's faster than `count()`.
*  `a_list.index(var)` returns the index of first occurrence of `var`. Optional parameters of start and end of search range can be specified. It throws an `ValueError` if `var` is not in `a_list`.
*  `del a_list[idx]` removes the `idx`th element. The gap is filled by shifting all elements on the right left by 1.
*  `a_list.remove(var)` removes first occurrence of `var`. It raises a `ValueError` if `var` is not in `a_list`.
*  `a_list.pop()` removes the last element in `a_list` and returns it. It raises and `IndexError` if the list is empty.
*  `a_list.sort()` sorts the list in-place, while `sorted(a_list)` makes a new list for sorted results. An optional parameter `key` specifies a **function** of one argument to extract the sorting key, and optional `reverse` makes the result in descending order if set to `True`, an optional `cmp` specifies a **function** of two arguments as a comparison function for more advanced custom sorting.

## Named Slices
```python
PRICE = slice(20,32)
record[PRICE] # same as record[20:32]
```
This is useful to name magic numbers.

## Use List as Stacks
The list methods make it very easy to use a list as a stack, where the last element added is the first element retrieved (“last-in, first-out”). To add an item to the top of the stack, use `append()`. To retrieve an item from the top of the stack, use `pop()` without an explicit index.

# Tuples
*Immutable* list. It provides a write-protect mechanism to your data.

Syntax: `a_tuple = ('a', 1)`, an empty tuple is `()`.

Note: an additional comma is required to create a tuple with exactly **ONE** elements. `a_tuple = (1)` assigns `1` to `a_tuple` (which is an `int`), but `a_tuple = (1,)` creates a tuple.

`[]` is used to index tuples. For example, `t[-1]` gets the last element in tuple `t`.

All **modification** methods in list are not valid. Access methods are still valid.

Tuples can be used to return multiple values from a function through **unpacking** (see [Python Iterator post for more information]({{ site.baseurl }}{% post_url /Python/2018-02-04-python-iterator %})):

An empty tuple is evaluated to `False`, non-empty tuples are evaluated to `True`.

```python
v = ('a', 2, True)
(x, y, z) = v # x == 'a', y == 2, z == True
```
So a function returns a tuple can actually assign multiple return values at once.

## Methods on lists, tuples, and strings
`len(s)`: returns the number of elements in `s`.

`min(s)`: returns the minimal element in `s`.

`max(s)`: returns the maximal element in `s`.

`all(s)`: `True` if all elements are evaluated to `True`.

`any(s)`: `True` if at least one element is evaluated to `True`.

## Methods on lists and tuples
`sum(x)`: returns sum of all elements in `s`.

`s.count(x)`: returns number of occurrences of `x` in `s`.

`s.index(x)`: returns the index of first occurrence of `x` in `s`.

# Set
An **unordered** bag of **unique** values.

Syntax: `a_set = {1}`. To create an empty set, we must call the constructor `set()`, because `{}` is an empty dictionary.

An empty set is evaluated to `False`.

Python set is implemented as dictionary with hash code as the key.

## Set Operations
* `a_set.add(var)`: add `var` to `a_set`. If `var` is already in `a_set`, nothing happens, it's just a no-op.
* `a_set.update(another_set)`: add all members in `another_set` to `a_set`. Duplications are ignored. `update()` can take multiple *sets, lists, and tuples* as parameters.
* To remove a `var` from `a_set`, we can use either `a_set.discard(var)` or `a_set.remove(var)`. The only difference is if `var in a_set == False`, `discard()` is a no-op, while `remove()` raises a `KeyError`.
* `a_set.pop()` removes a random value from `a_set` and returns it. To `pop()` from an empty set will raise a `KeyError` exception.
* `a_set.clear()` removes all members and leaves an empty set. It's equivalent to `a_set = set()`.
* **Union**: `a_set.union(b_set)` or `a_set | b_set`.
* **Intersection**: `a_set.intersection(b_set)` or `a_set & b_set`.
* **Difference**: `a_set.difference(b_set)` or `a_set - b_set`, it returns a set containing all the elements that are in `a_set` but not `b_set`.
* **Symmetric difference**: `a_set.symmetric_difference(b_set)` or `a_set ^ b_set`, it returns all the elements in *exactly one* of them.
* **Subset**: `a_set.issubset(b_set)`
* **Superset**: `a_set.issuperset(b_set)`

# Dictionary
An unordered set of key-value pairs. Dictionary is optimized to retrieve value with key, but no the other way around.

Syntax: `a_dict = {'key1': 'val1', 'key2': 'val2'}`. To retrieve a value, use `a_dict['key1']` to get `'val1'`. A `KeyError` will be raised if the given key is not present.

To add or change a key-value pair, use `a_dict['key3'] = 'val3'`.

To delete a key-value pair, use `del a_dict['key3']`.

A dictionary can be created from a list of 2-item lists.

An empty dictionary is evaluated to `False`.

`dic.keys()` returns all keys in `dic`.

`dic.values()` returns all values in `dic`.

`dic.items()` returns all key-value pairs (as tuples) in `dic`.

In Python2, `dic.keys()`, `dic.values()` and `dic.items()` are lists. In Python3, are view objects, meaning they'll reflect changes to `dic`.

`dict.fromkeys(seq, value=None)` creates a dictionary that maps items in `seq` to `value`, default to `None`. It is a class method.

## Default Values
The `defaultdict(default_factory)` method in `collections` module returns a `dict`-like object, with a factory method for missing values. For example, `defaultdict(list)` returns a dict that inserts an empty list when missing keys are accessed.

## Ordered Dict
The `OrderedDict` class from the `collections` module is a dict that remembers the order that keys were inserted. If a new entry overwrites an existing entry, the original insertion position is left unchanged. This is achieved by maintaining a doubly linked list that orders the keys according to insertion order. Be aware that the size of an OrderedDict is more than twice as large as a normal dictionary due to the extra linked list that’s created.

## Combined Dict
The `ChainMap` class in the `collections` module takes multiple mappings and makes them logically appear as one. However, the mappings are not literally merged together. Instead, a ChainMap simply keeps a list of the underlying mappings and redefines common dictionary operations to scan the list.

An alternative is to use `dict.update(another_dict)` method to actually perform the merge.

## How are dictionaries implemented in CPython?
CPython’s dictionaries are implemented as resizable hash tables. Compared to B-trees, this gives better performance for lookup (the most common operation by far) under most circumstances, and the implementation is simpler.

Dictionaries work by computing a hash code for each key stored in the dictionary using the hash() built-in function. The hash code varies widely depending on the key; for example, “Python” hashes to -539294296 while “python”, a string that differs by a single bit, hashes to 1142331976. The hash code is then used to calculate a location in an internal array where the value will be stored. Assuming that you’re storing keys that all have different hash values, this means that dictionaries take constant time – O(1), in computer science notation – to retrieve a key. It also means that no sorted order of the keys is maintained, and traversing the array as the .keys() and .items() do will output the dictionary’s content in some arbitrary jumbled order.

# Comprehension
## List comprehension
Syntax: `[<expression> for var in list_var if condition]`

`<expression>`: can be any Python expression, it will be the result of each element after list comprehension

`var`: temporary variable to hold a single variable in the list

`list_var`: the original list

`if condition`: optional, only the elements with `condition=true` will be kept in the final list

## Dictionary comprehension

Syntax: `{<key>:<value> for var in list_var if condition}`

Note: it is enclosed in `{}` and returns a dictionary

`<key>`: expression of keys in dictionary

`<value>`: expression of values in dictionary

Note: to get both keys and values from a dictionary, we can use `{... for key, value in dict.items()}`.

## Set comprehension
Syntax: `{<expression> for var in set_var if condition}`. Set comprehension can take set (as well as list) as input.

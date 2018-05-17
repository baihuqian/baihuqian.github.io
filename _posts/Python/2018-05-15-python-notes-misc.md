---
layout: "post"
title: "Python Notes: Misc"
date: "2018-05-15 22:05"
tags: Python
---

# Sort
Built-in function `sorted(iter)` returns a new sorted list from the items in iterable. It takes two optional parameters:

* `key`: a function to extract a comparison key.
* `reverse`: if set to true, comparison is reversed.

`list.sort()` sorts the list in place using `<`. Note `list.sort()` returns `None`.

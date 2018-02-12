---
layout: "post"
title: "Python: Notes: Serialization"
date: "2018-02-11 22:37"
tags: Python
---

There are two ways to serialize Python objects: binary or JSON.

# `pickle` module
Python standard library that can store native datatypes, lists/tuples/dictionaries/sets, and functions, classes and instance of classes.

## File
* `pickle.dump(data, file)` serializes a serializable Python data structure, `data`, to a binary writable file, `file`, using a Python-specific **binary** format, the pickle protocol. The protocol cannot cope with other programming language, and older version of Python may not support its latest version.
* `pickle.load(file)` loads a pickle file opened in binary read mode.
**DO NOT use pickle.load() on untrusted data!**

## Memory
* `pickle.dumps(data)` does the same thing except it returns a `byte` array.
* `pickle.loads(array)` loads a pickle byte array.

# `json` module
Python standard library to work with JSON file. Since JSON file must be encoded with Unicode, encoding should be specified when opening files (`encoding='utf-8'`).

Note: JSON doesn't support tuples and `bytes`. Lists are mapped as arrays, and dictionaries are mapped as objects.

`json.dump(data, file)`: same as `pickle.dump()` but it produces a JSON file.

* Optional `indent` argument, 0 means “put each value on its own line”, a number greater than 0 means “put each value on its own line, and use this number of spaces to indent nested data structures”.
* Optional `default` argument, a function that transforms the data before serialization. The function should transform an unsupported datatype into a dictionary, preferrably with the following format:

```python
  {'__class__': class_name,
   '__value__': value_transformation}
```

`json.load(file)` takes a stream object and creates a new Python object. To parse unsupported datatype, hook a transformation function to `object_hook` optional argument.

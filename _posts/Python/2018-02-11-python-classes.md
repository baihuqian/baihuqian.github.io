---
layout: "post"
title: "Python Notes: Classes"
date: "2018-02-11 10:58"
tags: Python
---

# Syntax
```python
class ClassName(SuperClass):
	'''docstring here'''

	class_var = "someValue"		# it's class level

	def __init__(self):
		pass

	startUp() 					# called once when imported
# outside class
```

* To *inherit* from a super class, enclose the super class in parenthesis between the class name and the comma.
* Class should have docstrings too.
* `__init__()` is **NOT** the constructor, but it is the first method called **AFTER** an instance of class is created.
* The first argument of every class method is `self`, referring to the current instance of the class. It's like `this` in Java, but it's not a reserved word, just a **STRONG** convention.
* Member variables are not defined directly (since Python variables cannot be declared without assigning a value to it). Instead, `self.var` defines a memeber variable `var`.
* Variables defined in class level (e.g. `class_var`) belongs to the class. Instance of a class can override this value, but it won't affect other instances. However, by modifying it on class level with `inst.__class__.class_var`, all instances with unmodified `class_var` will be affected.

# Instantiating Classes
Call the class by name as if it were a function, and pass parameters required by its `__init__()` function.

Every class instance has a built-in attribute, `__class__`, which is the object's class. `__doc__` is the docstring.

# Special methods
`x.__repr__()`: used by `repr(x)`, returns a printable representation of an object `x`. It should be a **valid Python expression**.

`x.__str__()`: used by `str(x)`, returns a nicely, string version of an object `x`. It is also called when you `print(x)`. If `x.__str__()` is not given, it falls back to `repr(x)`.

`x.__bytes__()`: similar, but returns a `bytes` object.

`x.__format__(format_spec)` is equivalent to `format(x, format_spec)`.

A class can implement other special methods to act like function, set, dictionary, number, or is comparable, serializable,

# Context manager
To define a context manager, define two special methods in a class: `__enter__()` and `__exit__()`.

`__enter__(self)` is called when entering a context (at the beginning of the `with` statement).

`__exit__(self, *args)` is called when exiting the context (at the end of the with statement).

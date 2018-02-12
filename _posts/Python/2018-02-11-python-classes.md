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

* Class should have docstrings too.
* `__init__()` is **NOT** the constructor, but it is the first method called **AFTER** an instance of class is created.
* The first argument of every class method is `self`, referring to the current instance of the class. It's like `this` in Java, but it's not a reserved word, just a **STRONG** convention.
* Member variables are not defined directly (since Python variables cannot be declared without assigning a value to it). Instead, `self.var` defines a memeber variable `var`.
* Variables defined in class level (e.g. `class_var`) belongs to the class. Instance of a class can override this value, but it won't affect other instances. However, by modifying it on class level with `inst.__class__.class_var`, all instances with unmodified `class_var` will be affected.

# Managed Attributes
Python attributes are normally defined as `self.x` fields in methods. However, there are ways to define attributes that are generated from a method.

```python
class Person:
	def __init__(self, first_name):
		self.first_name = first_name

	# Getter function
	@property
	def first_name(self):
		return self._first_name

	# Setter function
  @first_name.setter
	def first_name(self, value):
		if not isinstance(value, str):
			raise TypeError('Expected a string')
		self._first_name = value

  # Deleter function (optional)
  @first_name.deleter
	def first_name(self):
		raise AttributeError("Can't delete attribute")
```

The `@property` annotation defines a managed attribute. Use managed attributes for the following use cases:

* Getter performs extra computation, or the attribute is derived from other attributes.
* Setter performs extra validation.

Note that managed attributes are accessed as attributes, i.e. without parentheses. Note that extending managed attributes in subclasses are not the same as redefining them. Read more before implementing them.

# Instantiating Classes
Call the class by name as if it were a function, and pass parameters required by its `__init__()` function.

Every class instance has a built-in attribute, `__class__`, which is the object's class. `__doc__` is the docstring.

# Access Control
Although Python provides no language-level access control, there are two conventions to define non-public attributes.

* A single leading underscore (`_`) indicates the attribute is "internal", and should not be used outside the scope. Python does not prevent access to them, but it is considered a bad practice to use internal attributes.
* Two leading underscores (`__`) indicates the attribute is private and should not be overriden in inheritance. A `__x` attribute will be renamed to `_<class_name>__x` so that it cannot be overriden.

# Special methods
## String Representation
* `__repr__()`: used by `repr(x)`, returns a printable representation of an object `x`. It should be a **valid Python expression**. When formatting the string, the special `!r` formatting code can be used to indicate that the output of `__repr__()` should be used instead of `__str__()`, the default. For example, `'Pair({0.x!r}, {0.y!r})'.format(self)`.
* `__str__()`: used by `str(x)`, returns a nicely, string version of an object `x`. It is also called when you `print(x)`. If `__str__()` is not given, it falls back to `repr(x)`.

`x.__bytes__()`: similar, but returns a `bytes` object.

## String Formatting
Define the `__format__()` method. The `__format__()` method provides a hook into Python’s string formatting functionality. It will be invoked with `format(x, format_spec)`.

## Context Manager (`with` statement)
To define a context manager, define two special methods in a class: `__enter__()` and `__exit__()`.

`__enter__(self)` is called when entering a context (at the beginning of the `with` statement). The return value of `__enter__()` (if any) is placed into the variable indicated with the `as` qualifier.

`__exit__(self, *args)` is called when exiting the context (at the end of the with statement).

# Inheritance
To *inherit* from a super class, enclose the super class in parenthesis between the class name and the colon. Python supports multiple inheritance, and super classes are separated with comma:

```python
class C(A,B):
	pass
```

To call a method in a superclass, use the `super()` function:

```python
class A:
	def spam(self):
		print('A.spam')

class B(A):
	def spam(self):
		print('B.spam')
		super().spam() # Call parent spam()
```

Though in `B` it can call `A.spam(self)`, in diamond inheritance the method on the common super class will be invoked multiple times, while it will not using `super()`.

## Python's Implementation of Inheritance
For every class that you define, Python computes what’s known as a method resolution order (MRO) list. The MRO list is simply a linear ordering of all the base classes. The `__mro__` attribute for every class is the MRO list. To implement inheritance, Python starts with the leftmost class and works its way left- to-right through classes on the MRO list until it finds the first attribute match.
The actual determination of the MRO list itself is made using a technique known as C3 Linearization. It is actually a merge sort of the MROs from the parent classes subject to three constraints:

* Child classes get checked before parents
* Multiple parents get checked in the order listed.
* If there are two valid choices for the next class, pick the one from the first parent.

When you use the `super()` function, Python continues its search starting with the next class on the MRO. As long as every redefined method consistently uses `super()` and only calls it once, control will ultimately work its way through the entire MRO list and each method will only be called once. Thus, in multiple inheritance, if two unrelated superclasses define the same method, both of them will be invoked in a single `super()` method. Rule of thumb:

First, make sure that all methods with the same name in an inheritance hierarchy have a compatible calling signature (i.e., same number of arguments, argument names). This ensures that `super()` won’t get tripped up if it tries to invoke a method on a class that’s not a direct parent. Second, it’s usually a good idea to make sure that the topmost class provides an implementation of the method so that the chain of lookups that occur along the MRO get terminated by an actual method of some sort.

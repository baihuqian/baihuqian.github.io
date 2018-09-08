---
layout: "post"
title: "Python Notes: Functions"
date: "2018-02-11 10:55"
tags: Python
---

`def fun(parameter1, parameter2=1):`, all lines of function body are indented.

* Use keyword `def` to define functions.
* Parameters can have default values, and this parameter becomes optional.
* Python functions don't need to declare return types. If there are no explicit return values in function body, *`None`* is returned.
* Functions are also *objects*. So a function name contains not only the function declaration, but also its implementation with any parameters. And you can pass functions as parameters into any functions or store it in any data structures.

Though Python does not require declaring types of parameters and return values, they can be specified as *function argument annotations*:

```python
def add(x:int, y:int) -> int:
	return x + y
```

Note: they are not type checks, nor do they make Python behave any differently than it did before. Function annotations are merely stored in a function’s `__annotations__` attribute.

# Parameters
## Parameter Packing and Unpacking
If the number of parameters are not known when function is defined, we can use **packing**:

```python
def func(*name):
	doSomething()

func(1)
func(1,2,3)
```

Then `name` becomes a tuple of all parameters passed into `func`. `func` takes only **positional** arguments.

Or we can use dictionary:

```python
def func2(**dict):
	doSomething()

func2(a=1)
func2(a=1,b=2,c=3)
```

Then `dict` is a dictionary with key as argument name. `func` takes only **keyword** arguments.

Note: A `**` argument can only appear as the last argument, while a `*` argument can only appear as the last positional argument in a function definition. A subtle aspect of function definitions is that arguments can still appear after a `*` argument. This can be used to implement functions that only accept certain keyword arguments:

```python
def mininum(*values, clip=None): # clip is keyword-only
	m = min(values)
	if clip is not None:
	m = clip if clip > m else m
	return m

minimum(1, 5, 2, -5, 10) # Returns -5
minimum(1, 5, 2, -5, 10, clip=0) # Returns 0
```

`*` and `**` can be used as **unpacking**, i.e. pass a tuple (or a dictionary) to a function, and let each element in the tuple be a positional argument (or each key-value pair in the dictionary be a keyword argument). For example,

```python

def func(a, b, c)
	doSomething()

args = (1, 2, 3)
func(*args)		# 1 passed to a, 2 passed to b, etc

dict = {'a':1, 'b', 2, 'c', 3}
func(**dict)
```

## Default value
Defining a function with optional arguments is easy: simply assign values in the definition and make sure that default arguments appear last. However, there are two gotchas:

* If the default value is supposed to be a mutable container, such as a list, set, or dictionary, use `None` as the default value. This avoids unwanted behavior if the mutable container escapes the scope and get mutated.
* The default values assigned are bound only once at the time of function definition. If a local variable `x` is used as default value, the default value is assigned to the value of `x` at function definition, and does not change when `x` is changed.

# Return Values
To return multiple values from a function, simply return a tuple:

```python
def myfun():
    return 1, 2, 3

a, b, c = myfun()
```
Though it looks peculiar, but it's actually the comma that forms a tuple, not the parentheses.

# Function call
When calling a function, parameters can be:

* passed by value: `fun(1, 2)`;
* passed by name: `fun(parameter1 = 1, parameter2 = 2)`. In this case order doesn't matter. `fun(parameter2 = 2, parameter1 = 1)` is also valid.
* some passed by value, others passed by name: `fun(1, parameter2 = 2)`. Order matters for those passed by value, but order doesn't matter for those passed by name.
* **Pass-by-value should happen before pass-by-name.** For example, `fun(parameter1 = 1, 2)` is not valid.

# docstring
Documentation string is the first multiline string after the function definition. It is automatically assigned to `__doc__` variable of the function.

# Argument Annotation
Function argument annotations can be a useful way to give programmers hints about how a function is supposed to be used. For example, consider the following annotated function:

```python
def add(x:int, y:int) -> int:
	return x + y
```

The Python interpreter does not attach any semantic meaning to the attached annotations. They are not type checks, nor do they make Python behave any differently than it did before. Function annotations are merely stored in a function’s `__annotations__` attribute. However, they might give useful hints to others reading the source code about what you had in mind.

# Partially-Applied Functions
Sometimes a callable is passed into other functions as a parameter. However, the number of arguments may not match. `functools.partial()` can be used to create a partially-applied function from a function:

```python
from functools import partial

def spam(a, b, c, d):
	print(a, b, c, d)

s1 = partial(spam, 1) # a = 1
s2 = partial(spam, d=42) # d = 42
s3 = partial(spam, 1, 2, d=42) # a = 1, b = 2, d = 42
```

`partial()` applies positional arguments as well as keyword arguments.

# Generators
A function that contains the `yield` keyword will make it as a resumable function. Unlike a normal function, a generator only runs in response to iteration, so one can call `next(gen)` on it. The generator function pauses on **yield**. Repeated calling `next(generator)` will resume and pause again on **yield**.

**Generator saves CPU and RAM.**

```python
def make_counter(x)
	while True:
		yield x 				# not a normal function!
		x = x + 1

counter = make_counter(2)		# counter is a generator
next(counter)					# returns 2
next(counter)					# returns 3
```

Passing a generator to list/set/tuple constructor will generate a list/set/tuple with all values yield by the generator.

## `yield from` keyword
In Python3.3, `yield from` keyword is added. It basically let the generator yields value from another source, such as a generator or an iterator. For example,

```python
def generator():
    for i in range(10):
        yield i
```
becomes
```python
def generator():
	yield from range(10)
```

It enables some powerful use cases, such as depth-first search:
```python
class Node:
	def __init__(self, value):
		self._value = value
		self._children = []

	def add_child(self, node):
		self._children.append(node)

	def __iter__(self):
		return iter(self._children)

	def depth_first(self):
		yield self
		for c in self: #implicitly calls self.__iter__ for children
			yield from c.depth_first()
```

It allows chaining generators together:
```python
def gen_concatenate(iterators):
    from it in iterators:
        yield from it
```

## Generator expressions
A generator expression is like a generator function without the function. It looks like a list comprehension but is enclosed in parentheses (note there are no *tuple comprehensions*!).

Syntax: `(<expression> for var in list_var)`. It returns an **iterator**.

Generator expressions can be passed directly to a data reduction function (e.g., `sum()`, `min()`, `max()`):

```python
s = sum(x * x for x in nums)
```

This is equivalent to

```python
s = sum((x * x for x in nums)) # Pass generator-expr as argument
s = sum([x * x for x in nums]) # Use list comprehension
```

# `lambda` function
Syntax: `func = lambda x, y: x + y` defines a function `func` with argument `x` and `y`, and returns `x + y`.

Usage: create anonymous functions for functions like `filter()`, `map()` and `reduce()`:

* `filter(function, iter)`: returns an iterator with elements in `iter` evaluated as `True` by `function`.
* `map(function, iter)`: returns an iterator with return values by applying `function` on each element in `iter`.
* `reduce(function, iter)`: aggregates return values from `function` with each element in `iter` as argument.

If `lambda` expression refers to a free variable outside the function, the value of the free variable is bounded at runtime, not definition time. If you want an anonymous function to capture a value at the point of definition and keep it, include the value as a default value, like this:

```python
x = 10
a = lambda y: x + y # x is bounded at runtime
b = lambda y, x=x: x + y # the default value of x is bounded at definition time
```

The `x` in `b`'s body is a local variable with default value of the free variable `x`. Because default value are bound at definition time, it will not change at runtime.

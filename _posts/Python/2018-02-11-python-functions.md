---
layout: "post"
title: "Python Notes: Functions"
date: "2018-02-11 10:55"
tags: Python
---

# Syntax
`def fun(parameter1, parameter2=1):`, all lines of function body are indented.

* Use keyword `def` to define functions.
* Parameters can have default values, and this parameter becomes optional.
* Python functions don't need to declare return values. If there are no explict return values in function body, *`None`* is returned.
* Functions are also *objects*. So a function name contains not only the function declaration, but also its implementation with any parameters. And you can pass functions as parameters into any funtions or store it in any data structures.

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

`*` and `**` can be used as **unpacking**, i.e. pass a tuple (or a dictionary) to a function, and let each element in the tuple be a positional argument (or each key-value pair in the dictionary be a keyword argument). For example,

```python

def func(a, b, c)
	doSomething()

args = (1, 2, 3)
func(*args)		# 1 passed to a, 2 passed 2 b, etc

dict = {'a':1, 'b', 2, 'c', 3}
func(**dict)
```

# Function call
When calling a function, parameters can be:

* passed by value: `fun(1, 2)`;
* passed by name: `fun(parameter1 = 1, parameter2 = 2)`. In this case order doesn't matter. `fun(parameter2 = 2, parameter1 = 1)` is also valid.
* some passed by value, others passed by name: `fun(1, parameter2 = 2)`. Order matters for those passed by value, but order doesn't matter for those passed by name.
* **Pass-by-value should happen before pass-by-name.** For example, `fun(parameter1 = 1, 2)` is not valid.

# docstring
Documentation string is the first multiline string after the function definition. It is automatically assigned to `__doc__` variable of the function.

# Closures
You can build dynamic functions with a function by passing different parameters into it. This technique of using the values of outside parameters within a dynamic function is called *closures*. For example,

```python
import re

def build_match_and_apply_functions(pattern, search, replace):
	def matches_rule(word):
		return re.search(pattern, word)
	def apply_rule(word):
		return re.sub(search, replace, word)
	return (matches_rule, apply_rule)
```

the funtion `build_mach_and_apply_functions()` takes three parameters and build two dynamic functions `matches_rule()` and `apply_rules()`. The generated functions will contain outside parameters after they being generated.


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
A generator expression is like a generator function without the function. It looks like a list comprehension but is enclosed in parentheses (note there are no *tuple comrehensions*!).

Syntax: `(<expression> for var in list_var)`. It returns an iterator.


# `lambda` function
Syntax: `func = lambda x, y: x + y` defines a function `func` with argument `x` and `y`, and returns `x + y`.

Usage: create anonymous functions for functions like `filter()`, `map()` and `reduce()`:

* `filter(function, iter)`: returns an iterator with elements in `iter` evaluated as `True` by `function`.
* `map(function, iter)`: returns an iterator with return values by applying `function` on each element in `iter`.
* `reduce(function, iter)`: aggregates return values from `function` with each element in `iter` as argument.

# Decorator
Decorator can add additional functionality to existing functions. It takes a function and returns a new function:

```python
def decor(F):
	def new_F(a, b):
		print("input", a, b)
		return F(a, b)
	return new_F
```

So the decorator `decor` adds a `print()` before the execution of the original function `F`. To apply decoration, we can:

```python
@decor
def sum(a, b):
	return a + b
```

So now the function `sum()` will print out the input. It passes the original `sum()` function to the decorator and assign the return function to the name `sum`.

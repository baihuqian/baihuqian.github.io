---
layout: "post"
title: "Python Notes: Basics"
date: "2018-02-11 10:59"
tags: Python
---

* Everything is an object.
* Python source code is UTF-8 encoded by default. To change it, put an encoding declaration in the first line of the file: `# -*- coding: windows-1252 -*-`, or the second line if the first line is hash-bang.
* Install third-party library: in the root folder of the library, run the following command:

```bash
sudo python3 setup.py install
```

* To convert Python2 code to Python3, use `2to3`.


# Memory Management
Python is a dynamic-binding, object-oriented language, so variable names are just the references to actual objects.

To find what a variable refers, we can use `id(var)` to get the address of the referring object in memory (use `hex()` to convert the address to hexadecimal).

Python will cache integers and short strings and reuse them, so

```python
a = 1
b = 1
a is b		# True
a = 'good'
b = 'good'
a is b		# True
```

All objects in python have a **reference count**, we can use `sys.getrefcount(var)` to get it. Of note, when we pass `var` to `sys.getrefcount()`, a new temporary reference is created. Thus the result count is always 1 more than expected.

Python containers (lists, tuples, sets, dictionaries) can contain multiple elements of arbitrary type. However, containers contain only the reference to those objects, so the type doesn't really matter.

With containers two objects in Python can refer to each other, creating a so-called **reference cycle**.

When reference count decreases to 0, the object should be garbage collected. Python divides objects into three generations. One object passes through one garbage collection, its generation increases by 1. By assuming a long-living objects is less likely to be garbage-collected, long-living objects get scanned less by garbage colloctor.

`get_threshold()` method in `gc` module specifies garbage collection stretagy. It returns three values:

* First is threshold when garbage collection should be initiated.
* Second is how many garbage collection on Generation 0 should pass before that on Generation 1 should initiate.
* Third is how many garbage collection on Generation 1 should pass before that on Generation 2 should initiate.


# The Python language
`#` comments everything follows it until the end of line.

`\`: concatenate next line's code to this line (for multi-line coding)

`pass`: keyword that states "nothing here" and does nothing. It's just a placeholder. It's like an empty set of `{}` in Java.

## Built-in Functions
* `print(str)` prints `str` to `sys.stdout`. It takes an optional `file` parameter, which defaults to `sys.stdout`. When overriden, the output is redirected to the stream object. It takes additional `sep` and `end` parameters to specify different separation (default to `' '`) and end (default to `'\n'`).
* `input(str)` prints `str` as a hint and wait user to input a string. It returns the input as a string.
* `dir(obj)` prints all methods and variables of object `obj` (or class). It basically prints all the keys in an object's dict.
* `help(class)` goes to the manual of the class.
* `locals()` returns the current local symbol table.

## Assignment
Python uses `=` for assignment, but inline assignment is not supported.

Multiple assignments: `a, b = <exp_a>, <exp_b>`. It happens in parallel, so you can use `a, b = b, a` to swap values.

## Branches

```python
if i > 0:
	print('positive')
elif i < 0:
	print('negative')
else:
	print('zero')
```

Python uses `if... elif... else` for conditions, and there's a comma following the evaluation.

## Loop
### `for` loop
Syntax: `for var in iter`, where `iter` is an iterator.

To get Java-style loop over a range: `for(int i = 0; i < 5; i++)`, use `range(start, stop, step)` to generate an iterator of indexes.

To loop over an iterator with its index, use `enumerate()` (see [Python Iterator post for more information]({{ site.baseurl }}{% post_url /Python/2018-02-04-python-iterator %})):

```python
for idx, item in enumerate(iter):
```

### `while` loop
```python
while expression:
   statement(s)
```
Python does not support do-while loop, however, one can write one as follows:

```python
while True:
  stuff()
  if fail_condition:
    break
```

### Using `else` Statement with Loops

Python supports to have an else statement associated with a loop statement.

If the else statement is used with a for loop, the else statement is executed when the loop has exhausted iterating the list.

If the else statement is used with a while loop, the else statement is executed when the condition becomes false.

## Boolean operations
* `==` for comparison.
* `and` for AND, `or` for OR.
* `not` for negation.
* `is` for object identity.
* Comparison can be chained. E.g. `0 < n < 4000` works.


## Exceptions
Python uses `try...except...finally` block to handle exceptions:

```python
try:
	import chardet
except ImportError:
	chardet = None
finally:
	doSomething()
```

Exceptions are *raised* using the `raise` keywords. But functions do not declare exception they raise.

## Assertions
Syntax: `assert <expression> [, msg]`

If the expression evaluates to false, an AssertionError is raised with the optional `msg`.

## Evaluations
`eval(str)` can run a string of Python expression. It can be **DENERGEOUS** so don't run any untrusted strings!

`eval(str[, global, local])` takes two additional parameters that act as the global and local namespaces. Both of them are dictionaries which maps the variables in `str` (key) to the actual variables (value).

---
layout: "post"
title: "Python Notes: Decorators"
date: "2018-09-07 19:26"
tags: Python
---

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

However, the function metadata is lost by applying the decorator. For example, after applying `@decor`, the function metadata becomes:

```python
sum.__name__ 	# 'new_F'
countdown.__doc__ # None
countdown.__annotations__ # {}
```

Whenever you define a decorator, you should always remember to apply the `@wraps` decorator from the `functools` library to the underlying wrapper function.

```python
from functools import wraps

def decor(F):

	@wraps(F)
	def new_F(a, b):
		print("input", a, b)
		return F(a, b)
	return new_F
```

# Work with Function Signatures
A common practice of representing a function with arbitrary signature is using `*args, **kwargs` as parameters. For example, a time decorator that times the execution of a function:

```python
import time
from functools import wraps

def timethis(func):
  '''
  Decorator that reports the execution time.
  '''
  @wraps(func)
  def wrapper(*args, **kwargs):
    start = time.time()
    result = func(*args, **kwargs)
    end = time.time()
    print(func.__name__, end-start)
    return result

  return wrapper
```

# Decorators that Take Arguments
```python
from functools import wraps
import logging

def logged(level, name=None, message=None):
    '''
    Add logging to a function.  level is the logging
    level, name is the logger name, and message is the
    log message.  If name and message aren't specified,
    they default to the function's module and name.
    '''
    def decorate(func):
        logname = name if name else func.__module__
        log = logging.getLogger(logname)
        logmsg = message if message else func.__name__

        @wraps(func)
        def wrapper(*args, **kwargs):
            log.log(level, logmsg)
            return func(*args, **kwargs)
        return wrapper
    return decorate

# Example use
@logged(logging.DEBUG)
def add(x, y):
    return x + y

@logged(logging.CRITICAL, 'example')
def spam():
    print('Spam!')
```

The three nested function works as follows. The outermost function `logged()` accepts the desired arguments and simply makes them available to the inner functions of the decorator. The inner function `decorate()` accepts a function and puts a wrapper around it as normal. The key part is that the wrapper is allowed to use the arguments passed to `logged()`.

The decoration process evaluates as `logged(logging.DEBUG)(x, y)`. The result of `logged(logging.DEBUG)` must be a callable which, in turn, takes a function as input and wraps it.

# Decorators Inside Classes
Defining a decorator inside a class is straightforward, but you first need to sort out the manner in which the decorator will be applied. Specifically, whether it is applied as an instance or a class method.

```python
from functools import wraps

class A:
    # Decorator as an instance method
    def decorator1(self, func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            print('Decorator 1')
            return func(*args, **kwargs)
        return wrapper

    # Decorator as a class method
    @classmethod
    def decorator2(cls, func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            print('Decorator 2')
            return func(*args, **kwargs)
        return wrapper
```

A common confusion when writing decorators in classes is getting tripped up by the proper use of the extra `self` or `cls` arguments in the decorator code itself. Although the outermost decorator function, such as `decorator1()` or `decorator2()`, needs to provide a `self` or `cls` argument (since they're part of a class), the wrapper function created inside doesn’t generally need to include an extra argument. This is why the `wrapper()` function created in both decorators doesn’t include a self argument.

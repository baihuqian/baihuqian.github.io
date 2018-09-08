---
layout: "post"
title: "Python Notes: Classes"
date: "2018-02-11 10:58"
tags: Python
---

{:toc}

```python
class ClassName(SuperClass):
    '''docstring here'''

    class_var = "someValue" # it's class level

    def __init__(self):
        pass

    startUp() # called once when imported
# outside class
```

* Class should have docstrings too.
* `__init__()` is **NOT** the constructor, but it is the first method called **AFTER** an instance of class is created.
* The first argument of every class method is `self`, referring to the current instance of the class. It's like `this` in Java, but it's not a reserved word, just a **STRONG** convention.
* Member variables are not defined directly (since Python variables cannot be declared without assigning a value to it). Instead, `self.var` defines a memeber variable `var`.
* Variables defined in class level (e.g. `class_var`) belongs to the class. Instance of a class can override this value, but it won't affect other instances. However, by modifying it on class level with `inst.__class__.class_var`, all instances with unmodified `class_var` will be affected.

# Instantiating Classes
Call the class by name as if it were a function, and pass parameters required by its `__init__()` function. The instantiation operation (“calling” a class object) creates an empty object. When a class defines an `__init__()` method, class instantiation automatically invokes `__init__()` for the newly-created class instance.

Every class instance has a built-in attribute, `__class__`, which is the object's class. `__doc__` is the docstring.

## Alternative Constructors
To define a class with more than one constructor, you should use a class method.

```python
import time

class Date:
    # Primary constructor
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    # Alternate constructor
    @classmethod
    def today(cls):
        t = time.localtime()
        return cls(t.tm_year, t.tm_mon, t.tm_mday)

Date.today() # invocation of the alternative constructor
```
To use the alternate constructor, you simply call it as a function.

## Instantiate an Uninitialized Object
By calling `Date.__new__(Date)` on the `Date` class, you can create an uninitialized object of class `Date`, as `__init__()` is not invoked. Thus, it is now your responsibility to set the appropriate instance variables.

# Methods and Attributes
Because Python is OOP language, methods and attributes are objects as well. They're stored in a dict. Therefore, it is possible for an attribute to override method attributes with the same name.

## Managed Attributes
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

## Access Control
Although Python provides no language-level access control, there are two conventions to define non-public attributes.

* A single leading underscore (`_`) indicates the attribute is "internal", and should not be used outside the scope. Python does not prevent access to them, but it is considered a bad practice to use internal attributes.
* Two leading underscores (`__`) indicates the attribute is private and should not be overridden in inheritance. A `__x` attribute will be renamed to `_<class_name>__x` so that it cannot be overridden.

For most code, you should probably just make your nonpublic names start with a single underscore. If, however, you know that your code will involve subclassing, and there are internal attributes that should be hidden from subclasses, use the double underscore instead.

## Methods, Class Methods, and Static Methods
When defining methods in a class, there are two decorators to be applied to mark a method to be `@classmethod` or `@staticmethod`. The difference can be illustrated from the following example:

```python
class A(object):
    def foo(self, x):
        print "executing foo(%s,%s)"%(self, x)

    @classmethod
    def class_foo(cls, x):
        print "executing class_foo(%s,%s)"%(cls, x)

    @staticmethod
    def static_foo(x):
        print "executing static_foo(%s)"%x    

a = A()
```

Below is the usual way an object instance calls a method. The object instance, `a`, is implicitly passed as the first argument.

```python
a.foo(1)
# executing foo(<__main__.A object at 0xb7dbef0c>,1)
```

With class methods, the class of the object instance is implicitly passed as the first argument instead of `self`. You can also call `class_foo` using the class. In fact, if you define something to be a class method, it is probably because you intend to call it from the class rather than from a class instance. `A.foo(1)` would have raised a `TypeError`, but `A.class_foo(1)` works just fine:

```python
a.class_foo(1)
# executing class_foo(<class '__main__.A'>,1)

A.class_foo(1)
# executing class_foo(<class '__main__.A'>,1)
```

With static methods, neither `self` (the object instance) nor `cls` (the class) is implicitly passed as the first argument. They behave like plain functions except that you can call them from an instance or the class:

```python
a.static_foo(1)
# executing static_foo(1)

A.static_foo('hi')
# executing static_foo(hi)
```

Static methods are used to group functions which have some logical connection with a class to the class.

------------------------

To summarize, `foo` in class `A` is just a function, but when you call `a.foo` you don't just get the function `foo`, you get a **"partially applied"** version of the function with the object instance `a` bound as the first argument to the function. `foo` expects 2 arguments, while `a.foo` only expects 1 argument.

`a` is bound to `foo`. That is what is meant by the term "bound" below:

```python
print(a.foo)
# <bound method A.foo of <__main__.A object at 0xb7d52f0c>>
```

With `a.class_foo`, `a` is not bound to `class_foo`, rather the class `A` is bound to `class_foo`.

```python
print(a.class_foo)
# <bound method type.class_foo of <class '__main__.A'>>
```

Here, with a static method, even though it is a method, `a.static_foo` just returns a good old function with no arguments bound. `static_foo` expects 1 argument, and `a.static_foo` and `A.static_foo` expects 1 argument too.

```python
print(a.static_foo)
# <function static_foo at 0xb7d479cc>

print(A.static_foo)
# <function static_foo at 0xb7d479cc>
```

# Special Methods and Attributes
## Compact Memory Footprint
Python objects consists of dictionary. You can often greatly reduce the memory footprint of instances by adding the `__slots__` attribute to the class definition. When you define `__slots__`, Python uses a much more compact internal representation for instances. Instead of each instance consisting of a dictionary, instances are built around a small fixed-sized array, much like a tuple or list. Attribute names listed in the `__slots__` specifier are internally mapped to specific indices within this array. A side effect of using slots is that it is no longer possible to add new attributes to instances — you are restricted to only those attribute names listed in the `__slots__` specifier.

A common misperception of `__slots__` is that it is an encapsulation tool that prevents users from adding new attributes to instances. Although this is a side effect of using slots, this was never the original purpose.

## String Representation
* `__repr__()`: used by `repr(x)`, returns a printable representation of an object `x`. It should be a **valid Python expression**. When formatting the string, the special `!r` formatting code can be used to indicate that the output of `__repr__()` should be used instead of `__str__()`, the default. For example, `'Pair({0.x!r}, {0.y!r})'.format(self)`.
* `__str__()`: used by `str(x)`, returns a nicely, string version of an object `x`. It is also called when you `print(x)`. If `__str__()` is not given, it falls back to `repr(x)`.

`x.__bytes__()`: similar, but returns a `bytes` object.

## String Formatting
Define the `__format__(self, format_spec)` method. The `__format__()` method provides a hook into Python’s string formatting functionality, invoked by `format(obj, format_spec)`. The default `format_spec` is an empty string which usually gives the same effect as calling `str(value)`.

## Context Manager (`with` statement)
To define a context manager, define two special methods in a class: `__enter__()` and `__exit__()`.

`__enter__(self)` is called when entering a context (at the beginning of the `with` statement). The return value of `__enter__()` (if any) is placed into the variable indicated with the `as` qualifier.

`__exit__(self, *args)` is called when exiting the context (at the end of the with statement).

## Descriptor
A descriptor is a class that implements the three core attribute access operations (get, set, and delete) in the form of `__get__()`, `__set__()`, and `__delete__()` special methods. These methods work by receiving an instance as input. The underlying dictionary of the instance (`__dict__`) is then manipulated as appropriate. Here is an example:

```python
# Descriptor attribute for an integer type-checked attribute
class Integer:
    def __init__(self, name):
        self.name = name

    def __get__(self, instance, cls):
        if instance is None:
            return self
        else:
            return instance.__dict__[self.name]

    def __set__(self, instance, value):
        if not isinstance(value, int):
            raise TypeError('Expected an int')
        instance.__dict__[self.name] = value

    def __delete__(self, instance):
        del instance.__dict__[self.name]
```

To use a descriptor, instances of the descriptor are placed into a class definition as class variables. For example:
```python
class Point:
    x = Integer('x')
    y = Integer('y')
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

When you do this, all access to the descriptor attributes (e.g., x or y) is captured by the `__get__()`, `__set__()`, and `__delete__()` methods.

## Delegation
Delegation is a programming pattern where the responsibility for implementing a particular operation is handed off (i.e., delegated) to a different object. To implement such a way, we can implement `__getattr__()`, `__setattr()__`, or `__delattr__()` methods, which acts like catch-all for attribute lookup, modification, and deletion.

```python
class Proxy:
    def __init__(self, obj):
        self._obj = obj

    # Delegate attribute lookup to internal obj
    def __getattr__(self, name):
        print('getattr:', name)
        return getattr(self._obj, name)

    # Delegate attribute assignment
    def __setattr__(self, name, value):
        if name.startswith('_'):
            super().__setattr__(name, value)
        else:
            print('setattr:', name, value)
            setattr(self._obj, name, value)

    # Delegate attribute deletion
    def __delattr__(self, name):
        if name.startswith('_'):
            super().__delattr__(name)
        else:
            print('delattr:', name)
            delattr(self._obj, name)
```

When using delegation to implement proxies, there are a few additional details to note.

1. The `__getattr__()` method is actually a fallback method that only gets called when an attribute is not found. Thus, when attributes of the proxy instance itself are accessed (e.g., the `_obj` attribute), this method would not be triggered.
2. It is also important to emphasize that the  `__getattr__()` method usually does not apply to most special methods that start and end with double underscores.
2. The `__setattr__()` and `__delattr__()` methods need a bit of extra logic added to separate attributes from the proxy instance intelf and attributes on the internal object `_obj`. A common convention is for proxies to only delegate to attributes that don’t start with a leading underscore (i.e., proxies only expose the “public” attributes of the held instance).

## Comparable
To make objects of a class comparable using normal comparison operators, the class can support comparison by implementing a special method for each comparison operator. For example, to support the >= operator, you define a `__ge__()` method in the classes. Although defining a single method is usually no problem, it quickly gets tedious to create implementations of every possible comparison operator.

The `functools.total_ordering` class decorator can be used to simplify this process. To use it, you decorate a class with it, and define `__eq__()` and one other comparison method (`__lt__`, `__le__`, `__gt__`, or `__ge__`). The decorator then fills in the other comparison methods for you.

## Support Basic Operators
A class can implement special methods to support basic operators. For example, a class implements `__add__(self, other)` supports `+` operator.

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

* First, make sure that all methods with the same name in an inheritance hierarchy have a compatible calling signature (i.e., same number of arguments, argument names). This ensures that `super()` won’t get tripped up if it tries to invoke a method on a class that’s not a direct parent.
* Second, it’s usually a good idea to make sure that the topmost class provides an implementation of the method so that the chain of lookups that occur along the MRO get terminated by an actual method of some sort.

# Interface or Abstract Base Class
The usage of Interface or Abstract Base Class is to define a class from which you can perform type checking and ensure that certain methods are implemented in subclasses. To define an abstract base class, use the abc module. For example:

```python
from abc import ABCMeta, abstractmethod

class IStream(metaclass=ABCMeta):

    @abstractmethod
    def read(self, maxbytes=-1):
        pass

    @abstractmethod
    def write(self, data):
        pass
```

`@abstractmethod` must appear immediately before the function definition, if other decorators are used as well.

ABCs allow other classes to be registered as implementing the required interface, without actually inheriting from ABC. For example, you can do this:

```python
import io
# Register the built-in I/O classes as supporting our interface
IStream.register(io.IOBase)
```

# Mixin
Mixin classes appear in various places in the standard library, mostly as a means for extending the functionality of other classes. They are also one of the main uses of multiple inheritance. For instance, if you are writing network code, you can often use the `ThreadingMixIn` from the `socketserver` module to add thread support to other network-related classes. For example, here is a multithreaded XML-RPC server:

```python
from xmlrpc.server import SimpleXMLRPCServer
from socketserver import ThreadingMixIn
class ThreadedXMLRPCServer(ThreadingMixIn, SimpleXMLRPCServer):
    pass
```

Mixin classes are never meant to be instantiated directly. They have to be mixed with another class that implements the required functionality. Similarly, the `ThreadingMixIn` from the `socketserver` library has to be mixed with an appropriate server class - it can’t be used all by itself.

Mixin classes typically have no state of their own. This means there is no `__init__()` method and no instance variables. The specification of `__slots__ = ()` is meant to serve as a strong hint that the mixin classes do not have their own instance data.

Below are examples of mixins for dict:

```python
class LoggedMappingMixin:
    '''
    Add logging to get/set/delete operations for debugging.
    '''
    __slots__ = ()

    def __getitem__(self, key):
        print('Getting ' + str(key))
        return super().__getitem__(key)

    def __setitem__(self, key, value):
        print('Setting {} = {!r}'.format(key, value))
        return super().__setitem__(key, value)

    def __delitem__(self, key):
        print('Deleting ' + str(key))
        return super().__delitem__(key)

class SetOnceMappingMixin:
    '''
    Only allow a key to be set once.
    '''
    __slots__ = ()
    def __setitem__(self, key, value):
        if key in self:
            raise KeyError(str(key) + ' already set')
        return super().__setitem__(key, value)

class StringKeysMappingMixin:
    '''
    Restrict keys to strings only
    '''
    __slots__ = ()
    def __setitem__(self, key, value):
        if not isinstance(key, str):
            raise TypeError('keys must be strings')
        return super().__setitem__(key, value)

```

When using mixins, they're listed before the actual base class during inheritance.

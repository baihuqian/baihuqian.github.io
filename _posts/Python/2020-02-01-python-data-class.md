---
layout: "post"
title: "Python Data Class"
date: "2020-02-01 15:18"
tags: Python
---

Often we need a named data holder to store data according to certain schema. The object-oriented way is to create data classes with typed fields. For example, in Java we can create Java POJO, and with the help of [Lombok](https://projectlombok.org/features/Data), it is pretty simple:

```java
import lombok.Data;

@Data
public class Position {
    private String name;
    private int x;
    private int y;
}
```

In Scala, `case class` is your friend:

```scala
public case class Position(name: String, x: Int, y: Int)
```

But in Python, you either have to write a lot of boilerplate code (like Java without Lombok):

```python
class Position:
    def __init__(self, name, x, y):
        self.name = name
        self.x = x
        self.y = y

    def __repr__(self):
        return (f'{self.__class__.__name__}'
                f'(name={self.name!r}, x={self.x!r}, y={self.y!r})')

    def __eq__(self, other):
        if other.__class__ is not self.__class__:
            return NotImplemented
        return (self.name, self.x, self.y) == (other.name, other.x, other.y)

```

Or use a non-typed, flexible key-value data structure like `dict`:

```python
position = {'name': 'origin', 'x': 0, 'y': 0}
```

Neither is ideal. In Python 3.7, the `dataclass` annotation is introduced to simplify this:

```python
from dataclasses import dataclass

@dataclass
class Position:
    name: str
    x: int
    y: int
```

A data class comes with basic functionality, such as a sane `__repr__` and `__eq__`, already implemented. The `:` notation used for the fields is using a new feature in Python 3.6 called variable annotations.

It's easy to add default values to the data fields:

```python
from dataclasses import dataclass

@dataclass
class Position:
    name: str
    x: int = 0
    y: int = 0
```
This works exactly as if you had specified the default values in the definition of the `__init__` method of a regular class.

While type hints are optional, adding some kind of type hint is mandatory when defining the fields in your data class. Without a type hint, the field will not be a part of the data class. However, if you do not want to add explicit types to your data class, use `typing.Any`:

```python
from dataclasses import dataclass
from typing import Any

@dataclass
class WithoutExplicitTypes:
    name: Any
    value: Any = 42
```
While you need to add type hints in some form when using data classes, these types are not enforced at runtime.

Data classes are just regular classes, so you can add methods, override the default behavior, inherit from data classes, etc. quite freely.

The `@dataclass` annotation can take parameters to further enhance the functionality. The following parameters are supported:

* `init`: Add `__init__` method? (Default is `True`.)
* `repr`: Add `__repr__` method? (Default is `True`.)
* `eq`: Add `__eq__` method? (Default is `True`.)
* `order`: Add ordering methods? (Default is `False`.)
* `unsafe_hash`: Force the addition of a `__hash__` method? (Default is `False`.)
* `frozen`: If `True`, the data class is immutable (i.e. assigning to fields raise an exception). (Default is `False`.)

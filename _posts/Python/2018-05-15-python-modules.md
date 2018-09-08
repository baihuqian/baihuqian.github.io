---
layout: "post"
title: "Python Notes: Modules"
date: "2018-05-15 21:36"
tags: Python
---

# Modules
## `sys`
`sys.path`: Python's search path.

## `time` module
`time_struct` to represent a point in time (accurate to 1 millisecond).

`time.strptime(str)` takes a formatted time string and converts it to a `time_struct`.

Functions to convert between different time representations.

`time.localtime()`: converts Unix time to local time.

`time.sleep(sec)`: sleeps for some seconds.

## `subprocess` module

The subprocess module allows you to spawn new processes, connect to their input/output/error pipes, and obtain their return codes.

## `unittest` module
For unit testing in Python.

* To write a test case, subclass the `TestCase` of the `unittest` module.
* Each individual test is a method of the testing class. A test method takes no parameters, returns no value, and must have a name beginning with `test`.
* Tests pass if no exceptions are raised. Assertion is passed without raising any exceptions.
* TestCase class provides assertion methods. So call `self.assertXYZ()`.
* `unittest.main()` runs each test case.

`self.assertEqual(var1, var2)`: compares output with  expected value.
`self.assertRaises(exception, function, argument1, ...)`: test if `function` with given `arguments` will raise given `exception`.

### `xml.etree.ElementTree` module
The ElementTree library is part of the Python standard library, and is used to parse XML.

```python
import xml.etree.ElementTree as etree
```

`tree = etree.parse(file)` takes a filename or a stream object. It parses the whole file at once. It returns an object `tree` which represents the *entire* document.

`root = tree.getroot()` returns the root element in `tree`.

`tree.findall(queryStr)` is the same as `tree.getroot().findall(queryStr)`, just for convenience.

`etree.tostring(elem)` converts Element to XML string.

#### Element in ElementTree

* It evalues to `False` if it contains no children (i.e. `len(elem) == 0`).
* `elem.tag` is as `{namespace}localname`.
* It acts like a list, the items of the list are its **direct** children. So you can call `len()` on it, or use it as an iterator to loop throught all its child elements.
* `elem.attrib` is a dictionary containing its attributes.
* `elem.findall(queryStr)` takes a query string and returns a list of elements that match the query.
* `elem.find(queryStr)` takes a query string and returns the first matching element.
* To instantiate a new element, pass the element name as the first argument. Add attributes as optional argument `attrib`, which is a dictionary of attribute names and values.

Query string:
* `{namespace}localname` will search among the direct children of the given element.
* `//{namespace}localname` will search any elements regardless of nesting level.
* It's a "limit support for XPath expressions".

### `lxml` module
Third-party, install before using!

It provides 100% compatible ElementTree API with full XPath support.

It's faster on large files. If we only use ElementTree API, we can do this:

```python
try:
	from lxml import etree
except ImportError:
	import xml.etree.ElementTree as etree
```

### `chardet` module
For encoding detection, designed for Python 2.

### `random` module
* Set the random seed: `random.seed()`. It takes an integer, byte data, or use system clock if nothing is specified as the seed.
* Pick a random item from an iterable, use `random.choice(iter)`.
* Shuffle an iterable in place, use `random.shuffle(iter)`.
* Produce random integers from `start` to `stop` inclusive, use `random.randint(start, stop)`.
* Produce random numbers from 0 to 1, use `random.random()`.
* `random.uniform()` and `random.gauss()` generates uniformly and normally distributed values.

### `datetime` module
It is used for date and time manipulations. If it's not suffice, use `dateutil` module.

* `datetime.timedelta` represents a time interval.
* `datetime.datetime` represents a time.
* `datetime.strptime(str, format)` and `datetime.strftime(datetime_obj, format)` converts a string to a datetime object and vice versa.

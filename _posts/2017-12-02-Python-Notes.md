---
title: Python Notes
tags: Python
---



## Data Structure Libraries
### Heap/Priority Queue
`heapq` module provides an array-based implementation of **min** heap , where `heap[k] <= heap[2*k+1]` and `heap[k] <= heap[2*k+2]` for all `k`, counting elements from zero.

To create a heap, use a list initialized to [], or you can transform a populated list into a heap via function `heapify()`.

* `heapq.heappush(heap, item)`: push `item` into `heap`.
* `heapq.heappop(heap)`: pop and return the smallest item from the heap. To access the root of heap, use `heap[0]`.
* `heapq.heappushpop(heap, item)`: push then pop.
* `heapq.heapreplace(heap, item)`: pop then push.
* `heapq.heapify(x)`: transform list x into a heap, in-place, in linear time.

Some general-purpose methods based on heap:
* `heapq.merge(*iterables, key=None, reverse=False)`: Merge multiple sorted inputs into a single sorted output (for example, merge timestamped entries from multiple log files). Returns an iterator over the sorted values.
* `heapq.nlargest(n, iterable, key=None)` and `nsmallest` returns n largest/smallest items in a list from iterable. They perform best for smaller values of n. For larger values, it is more efficient to use the `sorted()` function. Also, when `n==1`, it is more efficient to use the built-in `min()` and `max()` functions.





# Standard input, output, and error
They are defined in `sys` module.

`sys.stdout` and `sys.stderr` are stream objects, but they are write-only. To redirect them to other streams, simply assign the target to them.

# HTTP Web Services
## Standard Python libraries

* `http.client` implements the low-level HTTP Protocol. You won't interact with it directly.
* `urllib.request`: standard API for accessing HTTP and FTP servers.
	* `urllib.request.urlopen(url)` takes a `url` string and returns a file-like object so that methods dealing with stream objects can be used.
	* `urllib.parse.urlencode(dic)` takes a dictionary and transforms it into a *URL-encoded* string. The string can be used as a payload in POST request.
	* It's very inefficient.

Note: HTTP servers don't deal with abstractions so data you get is in `bytes` format.

## Third Party

`httplib2`: The one to use.

* `http = httplib2.HTTP(dir)` `HTTP` object: the primary interface to `httplib2`. Pass a directory (caching) name to it (even it doesn't exist). You only need **ONE** `HTTP` object unless you're absolutely knows why.
* `httplib2.debuglevel` is 1 means printing all the data exchange between the server and local machine. After altering this value, create a **new** `HTTP` object.
* `http.request(url, type, payload)` issues an HTTP request for given `url`.
	* It returns two values: `response` contains all headers the server returned in a dictionary; `content` contains the actual data server returned in a `bytes` object.
	* The second parameter is the `type` of HTTP request, default is GET.
	* The third parameter is the `payload` to send to the server.
	* Optional `headers` parameter takes a dictionary with specified HTTP headers.
* `response.status` is the HTTP status code.
* `response.fromcache`tells if the response is from cache, not the server.
* `http.add_credentials(username, password, domain)` stores authentication information for a certain `domain` in `HTTP` object.


# Modules
## `sys`
`sys.path`: Python's search path.

## `time` module
`time_struct` to represent a point in time (accurate to 1 millisecond).

`time.strptime(str)` takes a formatted time string and converts it to a `time_struct`.

Functions to convert between different time representations.

`time.localtime()`: converts Unix time to local time.

`time.sleep(sec)`: sleeps for some seconds.

## `commands` module
Runs an external command.

`(status, output) = command.getstatusoutput(cmd)` runs the command and return the status code and output as a tuple.

`output = command.getoutput(cmd)` omits the status code.

Don't use `command.getstatus()`!!

`os.system(cmd)` dumps output to Python's stdout and returns the status code.

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
* Set the random seed: `random.seed()`. It takes an integer, byte data, or use system clock.
* Pick a random item from an iterable, use `random.choice(iter)`.
* Shuffle an iterable in place, use `random.shuffle(iter)`.
* Produce random integers from `start` to `stop` inclusive, use `random.randint(start, stop)`.
* Produce random numbers from 0 to 1, use `random.random()`.
* `random.uniform()` and `random.gauss()` generates uniformly and normally distributed values.

### `datetime` module
It is used for date and time manipulations. If it's not suffice, use `dateutil` module.

* `datetime.timedelta` represents a time interval.
* `datetime.datetime` represents a time.
* `datetime.strptime(str, format)` and datetime.strftime(datetime_obj, `format`) converts a string to a datetime object and vice versa.

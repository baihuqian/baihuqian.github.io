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



## Serialization
### `pickle` module
Python standard library that can store native datatypes, lists/tuples/dictionaries/sets, and functions, classes and instance of classes.

#### File
* `pickle.dump(data, file)` serializes a serializable Python data structure, `data`, to a binary writable file, `file`, using a Python-specific **binary** format, the pickle protocol. The protocol cannot cope with other programming language, and older version of Python may not support its latest version.
* `pickle.load(file)` loads a pickle file opened in binary read mode.
**DO NOT use pickle.load() on untrusted data!**

#### Memory
* `pickle.dumps(data)` does the same thing except it returns a `byte` array.
* `pickle.loads(array)` loads a pickle byte array.

## `json` module
Python standard library to work with JSON file. Since JSON file must be encoded with Unicode, encoding should be specified when opening files (`encoding='utf-8'`).

Note: JSON doesn't support tuples and `bytes`. Lists are mapped as arrays, and dictionaries are mapped as objects.

`json.dump(data, file)`: same as `pickle.dump()` but it produces a JSON file.

* Optional `indent` argument, 0 means “put each value on its own line”, a number greater than 0 means “put each value on its own line, and use this number of spaces to indent nested data structures”.
* Optional `default` argument, a function that transforms the data before serialization. The function should transform an unsupported datatype into a dictionary, preferrably with the following format:

```python
  {'__class__': class_name,
   '__value__': value_transformation}
```

`json.load(file)` takes a stream object and creates a new Python object. To parse unsupported datatype, hook a transformation function to `object_hook` optional argument.


# Working with files
`open(filename)`: global function that opens a file and returns a stream object.

It takes an optional `encoding` parameter to specify the encoding of the file. The default encoding is *platform dependent* and specified in `sys.getdefaultencoding()`, so **always specify encoding when reading/writing a file**. To get the default encoding, `import locale` and call `locale.getpreferredencoding()`.

An optional parameter `mode` specifies the file to be opened in mode:
* read: `'r'`, by default
* write `'w'`, will override the file. If the file doesn't exist, a new file is automatically created.
* exclusive write `'x'`, will create a file to write if it does not already exist. **Python3 specific**
* append `'a'`.
* text mode `'t'`, by default
* `'+'`: open a disk file for updating (reading and writing)
* If a bindary file is handled, include a `'b'` character in `mode`.

Python3 operates in "universal newline support" (specified as `'U'` in Python2), which maps all newlines to `\n` when reading, and maps all `\n` to system default when writing.

## `with` statement
```python
with open(filename, encoding = 'utf-8') as a_file
	doSomething()

doElse()			# a_file is closed
```

Use a `with` statement, which creates a runtime context. When exiting `with` block, the stream object `a_file` will automatically call its own `close()` method. The `as` clause assign the `with` context to a variable, but it's optional.

The `with` statement takes a comma-separated list of contexts acting like a series of nested `with` statements, so the context managers form a last-in-first-out stack.


## Binary file
When reading binary, it is important to stress that all data returned will be in the form of byte strings, not text strings. Similarly, when writing, you must supply data in the form of objects that expose data as bytes (e.g., byte strings, bytearray objects, etc.).

If you ever need to read or write text from a binary-mode file, make sure you remember to decode or encode it. For example:
```python
with open('somefile.bin', 'rb') as f:
    data = f.read(16)
    text = data.decode('utf-8')

with open('somefile.bin', 'wb') as f:
    text = 'Hello World'
    f.write(text.encode('utf-8'))
```

Objects such as arrays and C structures can be used for writing without any kind of intermediate conversion to a bytes object.
```python
import array
nums = array.array('i', [1, 2, 3, 4]) with open('data.bin','wb') as f:
    f.write(nums)
```

This applies to any object that implements the so-called “buffer interface,” which directly exposes an underlying memory buffer to operations that can work with it. Writing binary data is one such operation.

Many objects also allow binary data to be directly read into their underlying memory using the `readinto()` method of files. However, it is tricky to get it right as it is platform specific.

## Wrap a File Descriptor
A file descriptor is different than a normal open file in that it is simply an integer handle assigned by the operating system to refer to some kind of system I/O channel. `open(fd)`, where file descriptor is supplied as the first parameter to the `open()` function, wraps the file descriptor in a file object.

On Unix systems, this technique of wrapping a file descriptor can be a convenient means for putting a file-like interface on an existing I/O channel that was opened in a different way (e.g., pipes, sockets, etc.).

## Stream object
Many other datatypes can be turned into stream objects, not only files.
* `io.StringIO(str)` from `io` module turns a string to a stream object.
* `io.ByteIO(byte_array)` does the same thing to a `byte` array.

Operations:
* `a_file.read()` returns the whole content as a string. It takes an optional parameter to specify how many characters to read. Python does not consider reading past end-of-file to be an error; it simply returns an empty string.
* `a_file.readinto(arr)` fills the preallocated array `arr`. It works with the array module and numpy. Make sure to check its return value, which is the number of bytes actually read.
* `a_file.readline()` read one line.
* `a_file.readlines()` read all lines and return a list of each line.
* `a_file.write(str)` writes or appends `str` to the stream object.
* `a_file.seek(loc)` moves to a specific **byte** position in a file.
* `a_file.tell()` returns the current **byte** location.
* `a_file.close()` closes the file. `close()` an already closed file won't raise an exception.

* `name` is `filename` you pass into `open()`, without normalization to an absolute path.
* `encoding` is the encoding method. If a binary file is opened, there is no `encoding`.
* `mode` defaults to `r` (read), you can specify it by passing it into `open()`.

You can have a `for` loop to read the file line-by-line:
```python
for a_line in a_file:
	print(a_line)
```
It will automatically breaks `a_file` into lines. You might want to call `a_line.rstrip()` to get rid of tailing white spaces because `a_line` contains them.

## Compressed files
Python provides modules to read and write compressed files via stream objects. The `gzip` and `bz2` modules work with gzip and bz2 compressions. They provide an alternative `open()` implementation that has the same signature as regular one, so it can also work with `with` statement. However, if no mode is specified, it will always open file in **binary** mode. **Always specify file mode!**

Moreover, the file path in `gzip.open()` and `bz2.open()` can be a stream object, allowing it to work on various file-like objects such as sockets, pipes, and in-memory files.
```python
import gzip

f = open('somefile.gz', 'rb')
with gzip.open(f, 'rt') as g:
    text = g.read()
```

## Paths
Paths in Python always contains forward slashes, even in Windows.

### `os` module
* `os.getcwd()`: get current working directory
* `os.chdir()`: change working directory, can take abosulte paths and relative paths
* `os.listdir(path)`: returns a list of the names of the entries in `path` except `.` and `..`.
* `os.stat(filename)`: returns a file's metadata

`os.path` module:
* `os.path.basename(path)`: get the last component of the path (filename)
* `os.path.dirname(path)`: get the directory name (everything before filename)
* `os.path.join()`: take arbitrary number of parameters, join each part together with right slashes
* `os.path.expanduser()`: expand `~` in the parameter passed into this function
* `os.path.split(path)`: return a tuple of `(dirname, filename)` for the file directory and the file name
* `os.path.splitext(filename)`: return a tuple of `(name, extension)` of a filename, filename can include its directory.
* `os.path.realpath(relative_path)`: return the absolute path of a relative path
* `os.path.exists(path)`: test for the existence of a file.
* `os.path.isfile(path)`, `os.path.isdir(path)`, `os.path.islink(path)`: test if a path is a file, a directory, or a symlink
* `os.path.getsize(path)`, `os.path.getmtime(path)`: get the size and modification time

### `glob` module
Use shell-like wildcards.

`glob.glob(path_with_wildcards)`: returns a list of filenames (paths) matching this wildcard.

```python
import glob

pyfiles = glob.glob('somedir/*.py')
```

## `shutil` module
Supports high-level file operations such as copying and removal.

`shutil.copy(src, dst)` copies files.

## Temporary Files and Directories
The `tempfile` module has a variety of functions for performing this task. To make an unnamed temporary file, use `tempfile.TemporaryFile`.

The first argument to `TemporaryFile()` is the file mode, which is usually `w+t` for text and `w+b` for binary.

It also has `TemporaryDirectory`.

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

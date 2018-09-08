---
layout: "post"
title: "Python Notes: File I/O"
date: "2018-02-11 22:39"
tags: Python
---

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

# `with` statement
```python
with open(filename, encoding = 'utf-8') as a_file:
	doSomething()

doElse() # a_file is closed
```

Use a `with` statement, which creates a runtime context. When exiting `with` block, the stream object `a_file` will automatically call its own `close()` method. The `as` clause assign the `with` context to a variable, but it's optional.

The `with` statement takes a comma-separated list of contexts acting like a series of nested `with` statements, so the context managers form a last-in-first-out stack.


# Binary file
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
nums = array.array('i', [1, 2, 3, 4])
with open('data.bin','wb') as f:
    f.write(nums)
```

This applies to any object that implements the so-called “buffer interface,” which directly exposes an underlying memory buffer to operations that can work with it. Writing binary data is one such operation.

Many objects also allow binary data to be directly read into their underlying memory using the `readinto()` method of files. However, it is tricky to get it right as it is platform specific.

# Wrap a File Descriptor
A file descriptor is different than a normal open file in that it is simply an integer handle assigned by the operating system to refer to some kind of system I/O channel. `open(fd)`, where file descriptor is supplied as the first parameter to the `open()` function, wraps the file descriptor in a file object.

On Unix systems, this technique of wrapping a file descriptor can be a convenient means for putting a file-like interface on an existing I/O channel that was opened in a different way (e.g., pipes, sockets, etc.).

# Stream object
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

# Compressed files
Python provides modules to read and write compressed files via stream objects. The `gzip` and `bz2` modules work with gzip and bz2 compressions. They provide an alternative `open()` implementation that has the same signature as regular one, so it can also work with `with` statement. However, if no mode is specified, it will always open file in **binary** mode. **Always specify file mode!**

Moreover, the file path in `gzip.open()` and `bz2.open()` can be a stream object, allowing it to work on various file-like objects such as sockets, pipes, and in-memory files.
```python
import gzip

f = open('somefile.gz', 'rb')
with gzip.open(f, 'rt') as g:
    text = g.read()
```

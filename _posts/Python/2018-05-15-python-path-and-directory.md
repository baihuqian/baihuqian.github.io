---
layout: "post"
title: "Python Notes: Path and Directory"
date: "2018-05-15 21:38"
tags: Python
---

{:toc}

# Paths
Paths in Python always contains forward slashes, even in Windows.

## `os` module
* `os.getcwd()`: get current working directory
* `os.chdir()`: change working directory, can take abosulte paths and relative paths
* `os.listdir(path)`: returns a list of the names of the entries in `path` except `.` and `..`.
* `os.stat(filename)`: returns a file's metadata

## `os.path` module
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

## `glob` module
Use shell-like wildcards.

`glob.glob(path_with_wildcards)`: returns a list of filenames (paths) matching this wildcard.

```python
import glob

pyfiles = glob.glob('somedir/*.py')
```

## File Name Match `fnmatch`
`fnmatch` module provides utilities to do file name match for strings like in shell. `fnmatch(str, pattern)` follows the same rule as the underlying OS for case-sensitivity, while `fnmatchcase(str, pattern)` is always case-sensitive.

# `shutil` module
Supports high-level file operations such as copying and removal.

`shutil.copy(src, dst)` copies files.

# Temporary Files and Directories
The `tempfile` module has a variety of functions for performing this task. To make an unnamed temporary file, use `tempfile.TemporaryFile`.

The first argument to `TemporaryFile()` is the file mode, which is usually `w+t` for text and `w+b` for binary.

It also has `TemporaryDirectory`.

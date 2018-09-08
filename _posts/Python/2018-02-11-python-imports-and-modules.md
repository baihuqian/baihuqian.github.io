---
layout: "post"
title: "Python Notes: Imports and Modules"
date: "2018-02-11 11:00"
tags: Python
---

# Import
`import module`: include a module in the search path. Everything in the module can be accessed by the name `module`. During `import`, renaming is made possible by `as` clause. Without `as` clause, the module is accessed by the whole module name; otherwise it is accessed by the name provided with `as` clause. For example, in `import os`, the `os` module is used with the name `os`. In

```python
import xml.etree.ElementTree as etree
```

the module is accessed with the name `etree`.

You can import a part of a module (functions, classes) by

```python
from urllib.parse import urlencode
```

The function `urllib.parse.urlencode` can be accessed by `urlencode`.

`__name__`: if imported, it's the module's filename, without a directory path and extension. If it is run as a standalone program, it is `'__main__'`. **We can enclose test code for the module with** `if __name__ == '__main__':`.

`__import__(name)` takes a name of a module and returns an object of that module.

# Modules
A module can be a single Python file (in this case, module name is the file name), or a directory with an `__init__.py` (in this case, module name is the directory name). When Python sees an `__init__.py` in a directory, a multi-file module is created, and files within the directory can reference other files by *relative import*.

```python
from . import othermodule
```

The `.` and `..` syntax on the import statement might look funny, but think of it as specifying a directory name. `.` means look in the current directory and `..B` means look in the `../B` directory. This syntax only works with the `from` form of import.

`__init__.py` can contain nothing, some functions, or all functions. Mainly it is used as entry point of the module. If a module is spitted into multiple files but you want it to be a single coherent module, import everything from different files in the `__init__.py` file.

To unify separate directories under a common namespace, you organize the code just like a normal Python package, but you omit `__init__.py` files in the directories where the components are going to join together. To illustrate, suppose you have two different directories of Python code like this:
```
foo-package/
    spam/
        blah.py
bar-package/
    spam/
        grok.py
```

In these directories, the name spam is being used as a common namespace. Observe that there is no __init__.py file in either directory. This feature is known as "**namespace package**".

# Python Package Index
Python 3 comes with a packaging framework: Distutils.

Structure of a Distutils package is like this:

```bash
module_name/
|
+--README.txt
+--setup.py
+--module_name/
   |
   +--__init__.py
   +--othercode.py
```

`setup.py` carries information about the package that Distutils uses. `README.txt` should be Windows-compatile and contains introduction about the package. Use the following command to check `setup.py`:

```bash
python3 setup.py check
```

Many things can be done (packaging, publishing, etc.) by running `setup.py` with different parameters.

To include additional files such as documentations, a *manifest file* called `MANIFEST.in` should be placed in the root directory.

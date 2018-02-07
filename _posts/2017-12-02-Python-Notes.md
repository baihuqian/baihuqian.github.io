---
title: Python Notes
tags: Python
---

#Python General
* Everything is an object.
* Python source code is UTF-8 encoded by default. To change it, put an encoding declarartion in the first line of the file: `# -*- coding: windows-1252 -*-`, or the second line if the first line is hash-bang.
* Install third-party library: in the root folder of the library, run the following command:

```bash
sudo python3 setup.py install
```

* To convert Python2 code to Python3, use `2to3`.


# Memory Management
Python is a dynamic-binding, object-oriented language, so variable names are just the references to actual objects.

To find what a variable refers, we can use `id(var)` to get the adress of the referring object in memory (use `hex()` to convert the address to hexidecimal).

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
* `dir(obj)` prints all methods and variables of object `obj` (or class).
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

Python uses `if... elif... else` for conditions, an there's a comma following the evaluation.

## Loop
### `for` loop
Syntax: `for var in iter`, where `iter` is an iterator.

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

## Import
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

### Modules
A module can be a single Python file (in this case, module name is the file name), or a directory with an `__init__.py` (in this case, module name is the directory name). When Python sees an `__init__.py` in a directory, a multi-file module is created, and files within the directory can reference other files by *relative import*.

```python
from . import othermodule
```

`__init__.py` can contain nothing, some functions, or all functions. Mainly it is used as entry point of the module.

### Python Package Index
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

# Data Structure
`type()` returns the type of a variable.

`isinstance(var, type)` check whether `var` is of `type`.

Python variables cannot be declared without assigning a value to it.

## Boolean
`True` & `False`

Boolean values can be treated to numbers. `True` is 1, `False` is 0.

Equality: `==`.

To test for **identity**, use `is` keyword. E.g. `var1 is var2 == False`. **Identity means the two names refer to the same object.**

`any(iter)` returns `True` if any element of the iterable is true. `all(iter)` returns `True` if all elements of the iterable is true.

## Numbers
* `int`: can be arbitrarily large.
* `float`: is accurate to 15 decimal places. `int()` method truncates **towards 0**. `round(value, ndigits)` round towards nearest even digit (1.5 and 2.5 both round to 2), and `ndigits` can be negative (round to 10, 100, etc.).
* `fractions.Fraction(numerator, denominator)`. By `import fractions`, we can use the built-in fraction class. Note denominator cannot be 0.
* `math` package. It contains constants `math.pi`, all trigonometric functions.
* 0 and 0.0 are `False`, everything else is `True`. **Be careful about rounding error in floating points!!!**
* `complex(real, imag)` or `2 + 5j` generates complex number. All the usual number operations are supported, and use `cmath` module (instead of `math`) if necessary.
* `float('inf')`, `float('-inf')`, and `float('nan')` creates infinity, negative infinity and NaN. To check whether a value is inf or nan, use `math.isinf(a)` and `math.isnan(a)`. Note `==` and `is` cannot be used to check for nan!

### Number Operations
* `/`: floating point division, returns float no matter what.
* `//`: integer division. It always **truncates down (towards negative infinity)**, so `11//2 == 5`, `-11//2 == -6`.
* `**` power. `11 ** 2 == 121`.
* `%` remainder.

### Accurate Implementation
Floating point can't accurately represent all base-10 decimals. If needed (especially in finance world), use `decimal` module.

### Number Format
#### Float
Use built-in `format(value, definition)` function. `definition` is a string like `[<>^]?width[,]?(.digits)[fe]` where `width` is the width of the output, 0 means no override. `<` is left-justified, `>` is right-justitied, and `^` is centered. `digits` is the number of digits in accuracy. `f` means `float`, `e` or `E` means exponential notation. Place a `,` before `.` inserts thousands separator.

#### Integer
`bin(x)`, `oct(x)`, and `hex(x)` converts integer `x` into its binary, octal, and hexadecimal representation.

`format(x, 'b|o|x')` does the same thing, except it won't produce `0b|o|x` prefix.

Integer is signed, so negative value has a negative sign prefixing it. To convert to the unsigned representation, add the maximum number to it to set the bit length.

To convert back to decimal, use `int(str, base)`.

## None
`None` is Python's `null`.

Comparing `None` to anything other than `None` is always `False`.

`None` itself is evaluated to `False`.

## String
* Python3 string is UTF-8 encoded by default, Python2 is not. To change the encoding, use `s.encode('ascii')`. It takes optional parameters to keep unsupported charactors.
* String can be defined using single quotes (`'`) or double quotes(`"`).
* Prefix an `r` to a string makes it a *raw* string in Python. A raw string means Python will not escape anything.
* Multiline strings are enclosed in triple quotes (`'''` or `"""`).
* A string is like **a tuple of characters**. But strings can be concatenated using `+` operator.
* String cannot cancatenate with numbers without converting numbers to strings using `str()` first.
* String contains modification methods. They destory old strings and replace with a new, modified string.

`multiline_string.splitlines()`: returns a list of strings containing each line.

`s.split(char, [num])`: split string `s` from left according to the character `char` and return a list of substrings. Optional parameter `num` is the time you want to split. If `char` is `None`, it splits on any whitespaces.

`s.rsplit(char[, num])`: does the same thing except it starts from the right end.

`char.join(list_str)`: concatenates a list of strings in `list_str` with `char`.

`str.rstrip()` removes tailing whitespace, `str.lstrip()` removes leading whitespace, and `str.strip()` removes both. They can take other characters as parameters so they gets stripped.

`s.lower()` & `s.upper()`: converts `s` to lower or upper case. `s.swapcase` changes lower-case to upper-case and upper-case to lower-case.

`s.replace(old, new)`: create a **copy** of `s`and replace substring `old` in `s` with `new`.

`s.count(ss)`: count occurrence of a substring `ss` in `s`.

`s.find(ss)`: returns the index of first occurrence from left of a substring `ss`. If none exist, return -1. `s.rfind(ss)` searches from right.

`s.index(ss)` returns the index of first occurrence from left of a substring `ss`. If none exist, exception is raised. `s.rfind(ss)` searches from right.

`ord(char)`: returns the integer value of a character. It is the inverse of `chr()`.

`str.translate(trans_table)`: it takes a dictionary that maps the **byte representation** of one character to that of another, and replaces all occurrences of the keys of the translation table with corresponding values. To delete a character, map it to `None`.

`s.ljust(width)`, `s.rjust(width)`, `s.center(width)` aligns `s` to left, right, and center. It takes an optional fill character. It is the same as `format(s, '<20')`, `format(s, '>20')`, `format(s, '=^20')` (`'='` is the fill character). `format` works with more than strings.

`s.isalnum()` True if all characters are alphabet or numbers.

`s.isalpha()` True if all characters are alphabet.

`s.isdigit()` True if all characters are numbers.

`s.istitle()` True if all the first letters in all words are capitalized.

`s.isspace()` True if all characters are whitespaces.

`s.islower()` True if all characters are in lower case.

`s.isupper()` True if all characters are in upper case.

`s.startswith(('.c', '.h'))` True if `s` starts with either `'.c'` or `'.h'`. Same rule applies to `s.endswith()`. **Note here tuple is required as input!**

###`string.format`
* `a_string.format(var1, var2, ...)`: `a_string` contains placeholder in a format of `{i}`, i=0,1,..., and `{0}` is replaced by the first argument `var1`, `{1}` is replaced by `var2` etc. **We can skip the number `i` in curly brackets and just use `{}` for placeholder.
*** *Compound field names*:`var1` etc. can be a list too, so placeholder becomes `{0[0]}`, etc. The placeholder can be any datatypes.
* If `var1` is a dictionary, to access value from a string-type key, quotes a omitted, for example, `{0[key]}` instead of `{0['key']}`.
* *Format Specifiers*: e.g. `{0:.1f}`. A colon marks the start of the format specifier. For syntax, refer to [Format Specification Mini Language](file:///Users/baihu/Library/Application%20Support/Dash/DocSets/Python_3/Python%203.docset/Contents/Resources/Documents/doc/library/string.html#formatspec).

### Use `%` to format a string
Syntax:

```python
str % (var1, var2, ...)					# 1
str % {key1: val1, key2: val2, ...}		# 2
```

In 1, a template `str` is given, it's like C-type string formatter: `"I'm %s. I'm %d year old"`. A tuple of matching replacers are given follow the `%` operator.

In 2, a template `str` is given, placeholders are keys in the following dictionary. For example, `"I'm %(name)s. I'm %(age)d year old"` with the dictionary `{'name':'Vamei', 'age':99}`.


### File Name Match `fnmatch`
`fnmatch` module provides utilities to do file name match for strings like in shell. `fnmatch(str, pattern)` follows the same rule as the underlying OS for case-sensitivity, while `fnmatchcase(str, pattern)` is always case-sensitive.

### Unicode Normalization
A singble character can have multiple representations, namely composed (i.e., use a single code point if possible) and decomposed (with usage of combining characters).

`unicodedata` module provides a method `normalize(way, str)`. `way` specifies how you want to normalize `str`: `'NFC'` means fully composed, `'NFD'` means fully decomposed.

`combining(c)` method checks whether `c` is a combining character.

### Escape special characters in HTML
Use `html.escape(s)` in `html` module. To unescape it (given a bare text), use `unescape(s)` method in `HTMLParser` class in `html.parser` module. Normally you just need an HTML or XML parser.

### `textwrap` module
It provides a `fill(str, width)` method to convert a string to a multi-line string with given column width. Its optional parameter `initial_indent` and `subsequent_indent` specifies the indent for first and rest of lines, respectively.

To make it terminal-friendly, you can use `os.get_terminal_size().columns` to get the width of the terminal.

### Regular Expressions
Regular expressions are a powerful and (mostly) standardized way of searching, replacing, and parsing text with complex patterns of characters.

In Python, all functionality related to regular expression is in `re` module.

`re.compile(pattern[, flags])`: compiles a RegEx into an object. It is recommended to use raw Python strings `r'someString'` for `pattern` because it doesn't require escaping. It can contain an optional prefix `?P<PATTERNNAME>` to assign a name to the pattern.

There are some useful flags to customize the behavior of various methods.
* `re.IGNORECASE` or `re.I` enables case-insensitive text operations;
* `re.M` or `re.MULTILINE` enables multi-line environment so that `^` matches the beginning of the string and the beginning of the newline, and `$` matches the end of the string and the end of each line.
* `re.A` or `re.ASCII` Make \w, \W, \b, \B, \d, \D, \s and \S perform ASCII-only matching instead of full Unicode matching defaulted in Python 3.
* `re.S` or `re.DOTALL` let `.` match all characters including newline.
* `re.X` or `re.VERBOSE` enables  **verbose regular expressions**. In verbose regular expression, **whitespace** (spaces, tabs, carriage returns) and **comments** (everything follows a `#`) are ignored. To escape a whitespace, a `\` is required. We can write verbose regular expression as a multiline string, and comment every part of it in a separated line. For example, `a` and `b` are functionally the same:
```python
a = re.compile(r"""\d +  # the integral part
                   \.    # the decimal point
                   \d *  # some fractional digits
					""", re.X)
b = re.compile(r"\d+\.\d*")
```

Parentheses in pattern string define groups, which can be extracted by `group(id)` later. For example, `^(\d{3})-(\d{3,8})$` defines two groups. `group(0)` is the whole match, and `group(1)` and `group(2)` are the two matching groups.

By default, regex match is **greedy**, that is, it will match as many characters as possible. By adding a `?` to a pattern`, it becomes non-greedy, i.e. match as few characters as possible.

#### Functions for Regex
There are two ways to use functions in `re` module:
* `re.match(pattern, string[, flags])`
* `RegexObject.match(string,[ pos[, endpos]])`, and `RegexObject` is precompiled with `re.compile`.

If a match is found, a match object is returned; otherwise, `None` is returned. Match object is always evaludated to `True`, so we can do this:
```python
if re.search(regexp, exr):
	doSomething()
```

* `match` finds `pattern` from the **beginning** of the string. `RegexObject.match(string[, pos[, endpos]])` will match from `pos` instead of the beginning.
* `fullmatch` performs a match on the **whole string**.
* `search`: finds the first location of `pattern` in a string.
* `split`splits string by the occurrences of pattern and return a list. It takes an optional `maxsplit` parameter, default to 0. It is more flexible than `str.split` method.
* `findall`returns a list of all the substrings that matched the pattern. Note: it doesn't return **overlapping** matches.
* `finditer`returns an iterator of all matching substrings, instead of a list.
* `sub`: replaces **ALL** occurrences of pattern in string with replacement. If no occurrences are found, it acts like a no-op. `replacement` can be a function that takes **a single match object** and returns the replacement string.
* `subn` returns the number of matches too.

#### Matching Object
* `group(id)` returns one or more subgroups of the match. By default, `0` is the whole match, `1`, `2`, ... are matching groups defined by parentheses. If `?P<name>...` syntax (to define group name) is used, `name` can be passed in to `group` to get the match. It also takes `group(id1, id2, ...)` to return a tuple of matching groups.
* `groups` returns a tuple containing all the subgroups of the match, from 1 up. it takes a `default` parameter to define unfound match value (default to `None`).
* `groupdict`returns a dictionary containing all the **named** (`?P<name>...`) subgroups of the match, keyed by the subgroup name.
* `start` and `end`returns the indices of the start or end of the match. It takes the optional group ID.
* `span` returns the returns the indices of the start and end of the match in a tuple.

## Bytes
An *immutable* sequence of numbers between 0 and 255. To mutate a byte in `bytes` object, convert it to `bytearray` object. Indexing on it returns integers, not individual characters.

To create with literals, use `b'someString'`. To use constructors, use `bytes()`.

String and bytes are different datatypes, so they cannot be concatenated with each other.

Bytes can be converted to strings using `by.decode(encoding)` method. Similarly, `s.encode(encoding)` converts a UTF-8 string into bytes.

Bytes supports most of the same built-in  operations as text string, but not string formatting. Some of them also works with `bytearray` too. To use regular expression, pattern needs to be declared as byte string too.

## Collections
### Lists
Much like `ArrayList`, but it can hold multiple datatypes.

Sytax: `a_list = ['a', 1]`, square brackets with comma-separated list of values. To create a one-element list, use `[ val ]`. `list(val)` converts any iterable to a list, but it will fail if `val` is not iterable.

An empty list is evaluted to `False`, all non-empty list are `True`.

#### List Operations
* Index are zero-based.
* Negative index is counting backwwards from the end. Much like `a_list[-n] == a_list[len(a_list)-n]`.
* `a_list[begin:end]` is slicing. It returns a new list from `a_list[begin]` up but not including `a_list[end]`. If `begin == 0` and/or `end == len(a_list)`, they can be omitted, so `a_list[:]` returns a copy of `a_list`. Indices in slicing can also be negative.
* `+` can be used to concatnate lists.
* `a_list.append(var)` appends `var` to `a_list`'s end. It takes arbitrary types of variables and consider it as one. It returns `NoneType`.
* `a_list.extend(another_list)` concatnates `another_list` to the end of `a_list`.
* `a_list.insert(idx, var)` inserts `var` at `idx`th positions. Existing elements shifts right.
*  `a_list.count(var)` counts the number of occurrences of `var` in `a_list`. If `var` is not present, it returns 0.
*  `var in a_list` returns `True` if `a_list` contains `var`. It's faster than `count()`.
*  `a_list.index(var)` returns the index of first occurrence of `var`. Optional paramters of start and end of search range can be specified. It throws an `ValueError` if `var` is not in `a_list`.
*  `del a_list[idx]` removes the `idx`th element. The gap is filled by shifting all elements on the right left by 1.
*  `a_list.remove(var)` removes first occurrence of `var`. It raises a `ValueError` if `var` is not in `a_list`.
*  `a_list.pop()` removes the last element in `a_list` and returns it. It raises and `IndexError` if the list is empty.
*  `a_list.sort()` sorts the list in-place, while `sorted(a_list)` makes a new list for sorted results. An optional parameter `key` specifies a **function** of one argument to extract the sorting key, and optional `reverse` makes the result in descending order if set to `True`.

### Tuples
*Immutable* list. It provides a write-protect mechanism to your data.

Syntax: `a_tuple = ('a', 1)`, an empty tuple is `()`.

Note: an additional commma is required to create a tuple with exactly **ONE** elements. `a_tuple = (1)` assigns `1` to `a_tuple` (which is an `int`), but `a_tuple = (1,)` creates a tuple.

`[]` is used to index tuples. For example, `t[-1]` gets the last element in tuple `t`.

All **modification** methods in list are not valid. Access methods are still valid.

Tuples can be used to return multiple values from a function through multiple assignments.

```python
v = ('a', 2, True)
(x, y, z) = v # x == 'a', y == 2, z == True
```
So a function returns a tuple can actually assign multiple return values at once.

An empty tuple is evaluated to `False`, non-empty tuples are evaluated to `True`.

#### Methods on lists, tuples, and strings
`len(s)`: returns the number of elements in `s`.

`min(s)`: returns the minimal element in `s`.

`max(s)`: returns the maximal element in `s`.

`all(s)`: `True` if all elements are evaluated to `True`.

`any(s)`: `True` if at least one element is evaluated to `True`.

#### Methods on lists and tuples
`sum(x)`: returns sum of all elements in `s`.

`s.count(x)`: returns number of occurrences of `x` in `s`.

`s.index(x)`: returns the index of first occurrence of `x` in `s`.

### Set
An **unordered** bag of **unique** values.

Syntax: `a_set = {1}`. To create an empty set, we must call the constructor `set()`, because `{}` is an empty dictionary.

An empty set is evaluated to `False`.

Python set is implemented as dictionary with hash code as the key.

#### Set Operations
* `a_set.add(var)`: add `var` to `a_set`. If `var` is already in `a_set`, nothing happens, it's just a no-op.
* `a_set.update(another_set)`: add all members in `another_set` to `a_set`. Duplications are ignored. `update()` can take multiple *sets, lists, and tuples* as parameters.
* To remove a `var` from `a_set`, we can use either `a_set.discard(var)` or `a_set.remove(var)`. The only difference is if `var in a_set == False`, `discard()` is a no-op, while `remove()` raises a `KeyError`.
* `a_set.pop()` removes a random value from `a_set` and returns it. To `pop()` from an empty set will raise a `KeyError` exception.
* `a_set.clear()` removes all members and leaves an empty set. It's equivalent to `a_set = set()`.
* **Union**: `a_set.union(b_set)`.
* **Intersection**: `a_set.intersection(b_set)`.
* **Difference**: `a_set.difference(b_set)`, it returns a set containing all the elements that are in `a_set` but not `b_set`.
* **Symmetric difference**: `a_set.symmetric_difference(b_set)` or `a_set ^ b_set`, it returns all the elements in *exactly one* of them.
* **Subset**: `a_set.issubset(b_set)`
* **Superset**: `a_set.issuperset(b_set)`

### Dictionary
An unordered set of key-value pairs. Dictionary is optimized to retrieve value with key, but no the other way around.

Syntax: `a_dict = {'key1': 'val1', 'key2': 'val2'}`. To retrieve a value, use `a_dict['key1']` to get `'val1'`. A `KeyError` will be raised if the given key is not present.

To add or change a key-value pair, use `a_dict['key3'] = 'val3'`.

To delete a key-value pair, use `del a_dict['key3']`.

A dictionary can be created from a list of 2-item lists.

An empty dictionary is evaluated to `False`.

`dic.keys()` returns all keys in `dic`.

`dic.values()` returns all values in `dic`.

`dic.items()` returns all key-value pairs (as tuples) in `dic`.

In Python2, `dic.keys()`, `dic.values()` and `dic.items()` are lists. In Python3, are view objects, meaning they'll reflect changes to `dic`.

`dict.fromkeys(seq, value=None)` creates a dictionary that maps items in `seq` to `value`, default to `None`. It is a class method.

### Comprehension
#### List comprehension
Syntax: `[<expression> for var in list_var if condiition]`

`<expression>`: can be any Python expression, it will be the result of each element after list comprehension

`var`: temporary variable to hold a single variable in the list

`list_var`: the original list

`if condition`: optional, only the elements with `condition=true` will be kept in the final list

#### Dictionary comprehension

Syntax: `{<key>:<value> for var in list_var if condition}`

Note: it is enclosed in `{}` and returns a dictionary

`<key>`: expression of keys in dictionary

`<value>`: expression of values in dictionary

Note: to get both keys and values from a dictionary, we can use `{... for key, value in dict.items()}`.

#### Set comprehension
Syntax: `{<expression> for var in set_var if condition}`. Set comprehension can take set (as well as list) as input.

### Iterator
An iterator is just a **class** that defines an `__iter__(self)`  method. `__iter__(self)` function is automatically called when `iter(inst)` is called, where `inst` is an instance of this class. After performing beginning-of-iteration initialization, the `__iter__()` method *returns* any object that implements a `__next__()` method. Sometimes it just `return self`, because the class can implement `__next__()`. **`__iter__()`is a good place to initialize the iterator with initial values.**

The `__next__()` method is called when `next()` is called on an iterator of an instance of a class. It should raise a `StopIteration` exception to stop generating values. To spit out the next value, simply `return`s the value (do not use `yield`!).

An example:

```python
class Fib:
	''' Generate Fibonacci series '''

	def __init__(self, max):
		self.max = max		# class variable

	def __iter__(self):
		self.a = 0			# perform initialization
		self.b = 1
		return self

	def __next__(self):
		fib = self.a
		if fib > self.max
			raise StopIteration		# stop
		self.a, self.b = self.b, self.a + self.b
		return fib			# next value
```

To use iterator in `for` loop:

	for n in Fib(1000):
		doSomething...

`for` loop automatically creates the iterator, calls `next()` on it, and stop when `StopIteration` is raised.

There are several special methods with iterator:
* `__reversed__`: return a reversed iterator. The built-in `reversed()` function calls that. Reversed iteration only works if the object in question has a size that can be determined or `__reversed__`is implemented.
* `enumerate(iter)`returns an instance of an `enumerate` object, which is an iterator that returns successive tuples consisting of a counter(index) and the value returned by calling `next()` on the sequence you’ve passed in. It is useful to keep track of indexes in iteration.
* `zip(a, b)` creates an iterator that produces tuples `(x, y)` where x is taken from a and y is taken from b. One can iterate over multiple iterators at the same time. Iteration stops whenever one of the input sequences is exhausted. If this behavior is not desired, use `itertools.zip_longest()` instead. `zip` can also be used to create dictionary from two iterators: `dict(zip(headers,values))`.

#### `iter` method
It returns an `itertor` object.

* `iter(obj)`: `obj` must be a collection object that supports the iteration protocol or the sequence protocol, or `TypeError` is raised.
* `iter(callable, sentinel)`: it will call `callable` with no arguments for each call to its `__next__()` method; if the value returned is equal to `sentinel`, `StopIteration` will be raised, otherwise the value will be returned.

One useful application of the second form is to read lines of a file until a certain line is reached.

```python
with open('mydata.txt') as fp:
    for line in iter(fp.readline, ''):
        process_line(line)
```

#### `itertools` module
It provides many useful functions on iterators.
* `itertools.islice(iter, start, end)` slices an iterator. It achieves this by going through the iterator and discard unwanted items. `end` can be `None` if it returns everything beyond `start`.
* `itertools.dropwhile(func, iter)` discards the first items in `iter` as long as the supplied function returns `True`.
* `itertools.permutations(items, num)` generates permutations of `items` with length `num`. If `num` is not specified, the permutation is the same length of `items.`
* `itertools.combinations(items, num)` generates combinations. `itertools.combinations_with_replacement()` allows repetitions with same item in `items`.
* `itertools.chain()` chains multiple iterators together. It masks the actual type of each underlying iterators. It is more efficient than combining the sequences and then iterating.
* `itertools.product(a_list, b_list)`: returns an iterator over all Cartesian product of two sequences.
* `itertools.groupby(a_list, key)`: returns an iterator of iterators which groups elements in `a_list` by keys generated by `key` function. It only works if `a_list` is sorted by key.
* `itertools.zip_longest()`: does the same thing as the built-in `zip()` except it stops at the end of the *longest* sequence, padding `None` values for shorter sequences.

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

# Function
## Syntax
`def fun(parameter1, parameter2=1):`, all lines of function body are indented.

* Use keyword `def` to define functions.
* Parameters can have default values, and this parameter becomes optional.
* Python functions don't need to declare return values. If there are no explict return values in function body, *`None`* is returned.
* Functions are also *objects*. So a function name contains not only the function declaration, but also its implementation with any parameters. And you can pass functions as parameters into any funtions or store it in any data structures.

If the number of parameters are not known when function is defined, we can use **packing**:

```python
def func(*name):
	doSomething()

func(1)
func(1,2,3)
```

Then `name` becomes a tuple of all parameters passed into `func`. `func` takes only **positional** arguments.

Or we can use dictionary:

```python
def func2(**dict):
	doSomething()

func2(a=1)
func2(a=1,b=2,c=3)
```

Then `dict` is a dictionary with key as argument name. `func` takes only **keyword** arguments.

`*` and `**` can be used as **unpacking**, i.e. pass a tuple (or a dictionary) to a function, and let each element in the tuple be a positional argument (or each key-value pair in the dictionary be a keyword argument). For example,

```python
def func(a, b, c)
	doSomething()

args = (1, 2, 3)
func(*args)		# 1 passed to a, 2 passed 2 b, etc

dict = {'a':1, 'b', 2, 'c', 3}
func(**dict)
```

## Function call
When calling a function, parameters can be:

* passed by value: `fun(1, 2)`;
* passed by name: `fun(parameter1 = 1, parameter2 = 2)`. In this case order doesn't matter. `fun(parameter2 = 2, parameter1 = 1)` is also valid.
* some passed by value, others passed by name: `fun(1, parameter2 = 2)`. Order matters for those passed by value, but order doesn't matter for those passed by name.
* **Pass-by-value should happen before pass-by-name.** For example, `fun(parameter1 = 1, 2)` is not valid.

## docstring
Documentation string is the first multiline string after the function definition. It is automatically assigned to `__doc__` variable of the function.

## Closures
You can build dynamic functions with a function by passing different parameters into it. This technique of using the values of outside parameters within a dynamic function is called *closures*. For example,

```python
import re

def build_match_and_apply_functions(pattern, search, replace):
	def matches_rule(word):
		return re.search(pattern, word)
	def apply_rule(word):
		return re.sub(search, replace, word)
	return (matches_rule, apply_rule)
```

the funtion `build_mach_and_apply_functions()` takes three parameters and build two dynamic functions `matches_rule()` and `apply_rules()`. The generated functions will contain outside parameters after they being generated.


## Generators
A function that contains the `yield` keyword will make it as a resumable function. Unlike a normal function, a generator only runs in response to iteration, so one can call `next(gen)` on it. The generator function pauses on **yield**. Repeated calling `next(generator)` will resume and pause again on **yield**.

**Generator saves CPU and RAM.**

```python
def make_counter(x)
	while True:
		yield x 				# not a normal function!
		x = x + 1

counter = make_counter(2)		# counter is a generator
next(counter)					# returns 2
next(counter)					# returns 3
```

Passing a generator to list/set/tuple constructor will generate a list/set/tuple with all values yield by the generator.

### `yield from` keyword
In Python3.3, `yield from` keyword is added. It basically let the generator yields value from another source, such as a generator or an iterator. For example,

```python
def generator():
    for i in range(10):
        yield i
```
becomes
```python
def generator():
	yield from range(10)
```

It enables some powerful use cases, such as depth-first search:
```python
class Node:
	def __init__(self, value):
		self._value = value
		self._children = []

	def add_child(self, node):
		self._children.append(node)

	def __iter__(self):
		return iter(self._children)

	def depth_first(self):
		yield self
		for c in self: #implicitly calls self.__iter__ for children
			yield from c.depth_first()
```
It allows chaining generators together:
```python
def gen_concatenate(iterators):
    from it in iterators:
        yield from it
```

### Generator expressions
A generator expression is like a generator function without the function. It looks like a list comprehension but is enclosed in parentheses (note there are no *tuple comrehensions*!).

Syntax: `(<expression> for var in list_var)`. It returns an iterator.


## `lambda` function
Syntax: `func = lambda x, y: x + y` defines a function `func` with argument `x` and `y`, and returns `x + y`.

Usage: create anonymous functions for functions like `filter()`, `map()` and `reduce()`:

* `filter(function, iter)`: returns an iterator with elements in `iter` evaluated as `True` by `function`.
* `map(function, iter)`: returns an iterator with return values by applying `function` on each element in `iter`.
* `reduce(function, iter)`: aggregates return values from `function` with each element in `iter` as argument.

## Decorator
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


# Classes

## Syntax
```python
class ClassName(SuperClass):
	'''docstring here'''

	class_var = "someValue"		# it's class level

	def __init__(self):
		pass

	startUp() 					# called once when imported
# outside class
```

* To *inherit* from a super class, enclose the super class in parenthesis between the class name and the comma.
* Class should have docstrings too.
* `__init__()` is **NOT** the constructor, but it is the first method called **AFTER** an instance of class is created.
* The first argument of every class method is `self`, referring to the current instance of the class. It's like `this` in Java, but it's not a reserved word, just a **STRONG** convention.
* Member variables are not defined directly (since Python variables cannot be declared without assigning a value to it). Instead, `self.var` defines a memeber variable `var`.
* Variables defined in class level (e.g. `class_var`) belongs to the class. Instance of a class can override this value, but it won't affect other instances. However, by modifying it on class level with `inst.__class__.class_var`, all instances with unmodified `class_var` will be affected.

## Instantiating Classes
Call the class by name as if it were a function, and pass parameters required by its `__init__()` function.

Every class instance has a built-in attribute, `__class__`, which is the object's class. `__doc__` is the docstring.

## Special methods
`x.__repr__()`: used by `repr(x)`, returns a printable representation of an object `x`. It should be a **valid Python expression**.

`x.__str__()`: used by `str(x)`, returns a nicely, string version of an object `x`. It is also called when you `print(x)`. If `x.__str__()` is not given, it falls back to `repr(x)`.

`x.__bytes__()`: similar, but returns a `bytes` object.

`x.__format__(format_spec)` is equivalent to `format(x, format_spec)`.

A class can implement other special methods to act like function, set, dictionary, number, or is comparable, serializable,

## Context manager
To define a context manager, define two special methods in a class: `__enter__()` and `__exit__()`.

`__enter__(self)` is called when entering a context (at the beginning of the `with` statement).

`__exit__(self, *args)` is called when exiting the context (at the end of the with statement).

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

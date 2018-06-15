---
title: "Python Notes: String"
date: "2018-02-06 22:49"
tags: Python
---

Python3's string is UTF-8 encoded by default, Python2's is not. To change the encoding, use `s.encode('ascii')`. It takes optional parameters to keep unsupported characters.

String can be defined using single quotes (`'`) or double quotes(`"`). Multiline strings are enclosed in triple quotes (`'''` or `"""`). Prefix an `r` to a string makes it a *raw* string in Python. A raw string means Python will not escape anything.

A string is like **a tuple of characters**. Each character can be obtained by indexing. But strings can be concatenated using `+` operator. String cannot concatenate with numbers without converting numbers to strings using `str()` first. To convert a list of characters to string, use `''.join(char_list)`.

String provides modification methods. They destroy old strings and replace with a new, modified string. The commonly-used APIs are:

* `multiline_string.splitlines()`: returns a list of strings containing each line.
* `s.split(char, [num])`: split string `s` from left according to the character `char` and return a list of substrings. Optional parameter `num` is the time you want to split. If `char` is `None`, it splits on any whitespaces.
* `s.rsplit(char[, num])`: does the same thing except it starts from the right end.
* `char.join(list_str)`: concatenates a list of strings in `list_str` with `char`.
* `str.rstrip()` removes tailing whitespace, `str.lstrip()` removes leading whitespace, and `str.strip()` removes both. They can take other characters as parameters so they gets stripped.
* `s.lower()` & `s.upper()`: converts `s` to lower or upper case. `s.swapcase` changes lower-case to upper-case and upper-case to lower-case.
* `s.replace(old, new)`: create a **copy** of `s`and replace substring `old` in `s` with `new`.
* `s.count(ss)`: count occurrence of a substring `ss` in `s`.
* `s.find(ss)`: returns the index of first occurrence from left of a substring `ss`. If none exist, return -1. `s.rfind(ss)` searches from right.
* `s.index(ss)` returns the index of first occurrence from left of a substring `ss`. If none exist, `ValueError` exception is raised. `s.rindex(ss)` searches from right.
* `ord(char)`: returns the integer value of a character. It is the inverse of `chr()`.
* `str.translate(trans_table)`: it takes a dictionary that maps the **byte representation** of one character to that of another, and replaces all occurrences of the keys of the translation table with corresponding values. To delete a character, map it to `None`.
* `s.ljust(width)`, `s.rjust(width)`, `s.center(width)` aligns `s` to left, right, and center. It takes an optional fill character. It is the same as `format(s, '<20')`, `format(s, '>20')`, `format(s, '=^20')` (`'='` is the fill character). `format` works with more than strings.
* `s.isalnum()` True if all characters are alphabet or numbers.
* `s.isalpha()` True if all characters are alphabet.
* `s.isdigit()` True if all characters are numbers.
* `s.istitle()` True if all the first letters in all words are capitalized.
* `s.isspace()` True if all characters are whitespaces.
* `s.islower()` True if all characters are in lower case.
* `s.isupper()` True if all characters are in upper case.
* `s.startswith(('.c', '.h'))` True if `s` starts with either `'.c'` or `'.h'`. Same rule applies to `s.endswith()`. **Note here tuple is required as input!**

## String Formatting
There are two ways of formatting a string.

### `str.format` Method
`a_string.format(var1, var2, ...)`: format string with placeholders.

`a_string` contains placeholder in a format of `{i}`, `i=0,1,...`, and `{0}` is replaced by the first argument `var1`, `{1}` is replaced by `var2` etc. **We can skip the number `i` in curly brackets and just use `{}` for placeholder.** `var1` etc. can be a list too, so placeholder becomes `{0[0]}`, etc. The placeholder can be any datatypes. If `var1` is a dictionary, to access value from a string-type key, quotes a omitted, for example, `{0[key]}` instead of `{0['key']}`. Format Specifiers, e.g. `{0:.1f}`, can be used to specify the format of the variable. A colon marks the start of the format specifier.

### `%` Operator
Use `%` to format a string:

```python
str % (var1, var2, ...)  # 1
str % {key1: val1, key2: val2, ...}  # 2
```

In 1, a template `str` is given, it's like C-type string formatter: `"I'm %s. I'm %d year old"`. A tuple of matching replacers are given follow the `%` operator.

In 2, a template `str` is given, placeholders are keys in the following dictionary. For example, `"I'm %(name)s. I'm %(age)d year old"` with the dictionary `{'name':'Vamei', 'age':99}`.

### Unicode Normalization
A single character can have multiple representations, namely composed (i.e., use a single code point if possible) and decomposed (with usage of combining characters).

`unicodedata` module provides a method `normalize(way, str)`. `way` specifies how you want to normalize `str`: `'NFC'` means fully composed, `'NFD'` means fully decomposed.

`combining(c)` method checks whether `c` is a combining character.

### Escape special characters in HTML
Use `html.escape(s)` in `html` module. To unescape it (given a bare text), use `unescape(s)` method in `HTMLParser` class in `html.parser` module. Normally you just need an HTML or XML parser.

### `textwrap` module
It provides a `fill(str, width)` method to convert a string to a multi-line string with given column width. Its optional parameter `initial_indent` and `subsequent_indent` specifies the indent for first and rest of lines, respectively.

To make it terminal-friendly, you can use `os.get_terminal_size().columns` to get the width of the terminal.

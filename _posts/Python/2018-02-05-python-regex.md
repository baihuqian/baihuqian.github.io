---
layout: "post"
title: "Python Notes: RegEx"
date: "2018-02-05 23:05"
tags: Python
---

Regular expressions are a powerful and (mostly) standardized way of searching, replacing, and parsing text with complex patterns of characters.

In Python, all functionality related to regular expression is in `re` module.

`re.compile(pattern[, flags])`: compiles a RegEx into an object. It is recommended to use raw Python strings `r'someString'` for `pattern` because it doesn't require escaping. It can contain an optional prefix `?P<PATTERNNAME>` to assign a name to the pattern.

There are some useful flags to customize the behavior of various methods.
* `re.IGNORECASE` or `re.I` enables case-insensitive text operations;
* `re.M` or `re.MULTILINE` enables multi-line environment so that `^` matches the beginning of the string and the beginning of the newline, and `$` matches the end of the string and the end of each line.
* `re.A` or `re.ASCII` Make `\w`, `\W`, `\b`, `\B`, `\d`, `\D`, `\s` and `\S` perform ASCII-only matching instead of full Unicode matching defaulted in Python 3.
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

If a match is found, a match object is returned; otherwise, `None` is returned. Match object is always evaluated to `True`, so we can do this:
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
* `groupdict` returns a dictionary containing all the **named** (`?P<name>...`) subgroups of the match, keyed by the subgroup name.
* `start` and `end` returns the indices of the start or end of the match. It takes the optional group ID.
* `span` returns the indices of the start and end of the match in a tuple.

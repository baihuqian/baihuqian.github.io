---
layout: "post"
title: "Python Notes: Basic Types"
date: "2018-02-11 10:50"
tags: Python
---

`type()` returns the type of a variable.

`isinstance(var, type)` check whether `var` is of `type`.

Python variables cannot be declared without assigning a value to it.

* TOC
{:toc}

# Boolean
`True` & `False`

Boolean values can be treated to numbers. `True` is 1, `False` is 0.

Equality: `==`.

To test for **identity**, use `is` keyword. E.g. `var1 is var2 == False`. **Identity means the two names refer to the same object.**

`any(iter)` returns `True` if any element of the iterable is true. `all(iter)` returns `True` if all elements of the iterable is true.

# Numbers
* `int`: can be arbitrarily large.
* `float`: is accurate to 15 decimal places. `int()` method truncates **towards 0**. `round(value, ndigits)` round towards nearest even digit (1.5 and 2.5 both round to 2), and `ndigits` can be negative (round to 10, 100, etc.).
* `fractions.Fraction(numerator, denominator)`. By `import fractions`, we can use the built-in fraction class. Note denominator cannot be 0.
* `math` package. It contains constants `math.pi`, all trigonometric functions.
* 0 and 0.0 are `False`, everything else is `True`. **Be careful about rounding error in floating points!!!**
* `complex(real, imag)` or `2 + 5j` generates complex number. All the usual number operations are supported, and use `cmath` module (instead of `math`) if necessary.
* `float('inf')`, `float('-inf')`, and `float('nan')` creates infinity, negative infinity and NaN. To check whether a value is inf or nan, use `math.isinf(a)` and `math.isnan(a)`. Note `==` and `is` cannot be used to check for nan!

## Number Operations
* `/`: floating point division, returns float no matter what.
* `//`: integer division. It always **truncates down (towards negative infinity)**, so `11//2 == 5`, `-11//2 == -6`.
* `**` power. `11 ** 2 == 121`.
* `%` remainder.

## Accurate Implementation
Floating point can't accurately represent all base-10 decimals. If needed (especially in finance world), use `decimal` module.

## Number Format
### Float
Use built-in `format(value, definition)` function. `definition` is a string like `[<>^]?width[,]?(.digits)[fe]` where `width` is the width of the output, 0 means no override. `<` is left-justified, `>` is right-justitied, and `^` is centered. `digits` is the number of digits in accuracy. `f` means `float`, `e` or `E` means exponential notation. Place a `,` before `.` inserts thousands separator.

### Integer
`bin(x)`, `oct(x)`, and `hex(x)` converts integer `x` into its binary, octal, and hexadecimal representation.

`format(x, 'b|o|x')` does the same thing, except it won't produce `0b|o|x` prefix.

Integer is signed, so negative value has a negative sign prefixing it. To convert to the unsigned representation, add the maximum number to it to set the bit length.

To convert back to decimal, use `int(str, base)`.

# None
`None` is Python's `null`.

Comparing `None` to anything other than `None` is always `False`.

`None` itself is evaluated to `False`.

# Bytes
An *immutable* sequence of numbers between 0 and 255. To mutate a byte in `bytes` object, convert it to `bytearray` object. Indexing on it returns integers, not individual characters.

To create with literals, use `b'someString'`. To use constructors, use `bytes()`.

String and bytes are different datatypes, so they cannot be concatenated with each other.

Bytes can be converted to strings using `by.decode(encoding)` method. Similarly, `s.encode(encoding)` converts a UTF-8 string into bytes.

Bytes supports most of the same built-in  operations as text string, but not string formatting. Some of them also works with `bytearray` too. To use regular expression, pattern needs to be declared as byte string too.

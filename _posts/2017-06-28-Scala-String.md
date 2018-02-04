---
title: Learning Scala - String
tags: Scala
---

Scala `String` is the Java String class. String literals are enclosed with double quotes (`"`). Multi-line string literals are enclosed in triple double quotes:

```scala
val s = """This is 
    a multi-line
    string"""
```
The above string will have empty space at the beginning of 2nd and 3rd line. To strip them, use `stripMargin` and `|` (or any character by passing it into `stripMargin` method) as follows:
```scala
val s = """This is 
    |a multi-line
    |string""".stripMargin
```

Multi-line string literal also escapes `'` and `"` automatically.

`StringOps` offers more methods on `String` via *implicit conversion*, such as treating a `String` as a sequence of characters in a `for` loop or use `map` on them. To explicitly convert a `String` object to a collection type, use `toArray` or `toList` method. 

## String substitution
Prepending `s` to any string literal allows the usage of variables directly in the string.

`s"Hello, $name"` interpolates the string with variable `name`. String interpolators can also take arbitrary expressions, and they should be enclosed in `${}`.

If the string contains escaped double quotes, use triple quotes instead: `s """This is a quote: \"$quote\""""`.

#### `printf` style formatting

Prepending `f` to any string literal allows the creation of simple formatted strings, similar to printf in other languages. When using the `f` interpolator, all variable references should be followed by a `printf`-style format string, like `%d`. For example, `f"$name%s is $height%2.2f meters tall"`.

The `f` interpolator is typesafe. If you try to pass a format string that only works for integers but pass a double, the compiler will issue an error.

## The `raw` Interpolator

The `raw` interpolator is similar to the `s` interpolator except that it performs no escaping of literals within the string. 

## Useful APIs
* `capitalize`: convert a string to upper case.
* `split`: takes a String, a Char, or regular expressions and splits the string.
* `replaceAll`: It replaces all occurrences (can be specified with regex) with another string. `replaceFirst` does the same for first occurrence only.

## Regular Expressions
`scala.util.matching.Regex` class provides the functionality. To construct a regex object, usr `r` method of the String class. You need to use raw string `"""..."""` to avoid escaping backslashes:

```scala
val pattern = """\s+[0-9]+\s+""".r
```
* `pattern.findAllIn(str)` to find all matches in `str`. It returns an iterator.
* `pattern.findFirstIn(str)` to find the first match and return an `Option[String]`.
* `pattern.findPrefixOf(str)` to check whether the beginning of the given string matches.

Use `replace` methods (such as `replaceAllIn`) to replace all matches in a given string with another string:

```scala
pattern.replaceAllIn(str, rep)
```

### Extract matched patterns
Use groups to extract subexpressions from matches. Put parentheses around the subexpression to form a group. For example,

	val pattern = "([0-9+]) ([a-z]+)".r

Then `val pattern(num, item) = str` assign `num` to match of `[0-9+]` and `item` to match of `[a-z]+`. You can think of this as `unapply` method.
	

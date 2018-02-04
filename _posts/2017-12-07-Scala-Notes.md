---
title: Scala Notes
tags: Scala
---

# General
As a language that supports functional programming, Scala encourages an expression-oriented programming (EOP) model. That is, every statement (expression) yields a value.

Scala interpreter (invoked by `scala` command or `console` command in sbt) is not actually an interpreter. It compiles expression into Java bytecode and runs in JVM. It is referenced as *read-eval-print loop*, or *REPL* in short.

No semicolons at the end lines. Semicolons are only needed if you have multiple statements on the same line.

## Identifier
All names are called identifiers.

Identifiers can use:

* Unicode characters
* !#%&*+-/:<=>?@\^|~

Backquotes forms an escape hatch. `` `yield` `` is no longer a keyword in Scala.

## Operators
Infix operators: `a identifier b`, such as `1 to 10, 1-> 10`. They're binary operators.

Unary operators: `a identifier` is the same as `a.identifier()`. For example, `1 toString`.

`+,-,!,~` are allowed as *prefix* operators. So be careful when breaking code into multiple lines at `+` and `-`: They have to be at the **end** of the line, not the beginning of the line!

```scala
x
+y
```
evaluates to `y`, but
```scala
x +
y
```
evaluates to `x + y`.

Assignment operators: have the form `operator=`. `a operator= b` is the same as `a = a operator b`. `<=`, `>=`, `!=` are not. Scala does not have ++ or -- operators, but it has += and -=.

### The `apply` method
`apply` serves the purpose of closing the gap between Object-Oriented and Functional paradigms in Scala. Every function in Scala can be represented as an object. Every function also has an OO type: for instance, a function that takes an Int parameter and returns an Int will have OO type of `Function1[Int,Int]`.

Now if we want to actually execute the function, or as mathematician say "apply a function to its arguments" we would call the apply method on the `Function1[Int,Int]` object.

Every function in Scala can be treated as an object and it works the other way too - every object can be treated as a function, provided it has the `apply` method.
The `apply` method: `f.apply(arg1, arg2, ...)` is the same as `f(arg1, arg2, ...)`.

The `update` method: `f(arg1, arg2, ...) = value` is the same as `f.update(arg1, arg2, ..., value)`.

Extractor: `unapply` method that retrieves values from an object. For example,

```scala
val author = "Baihu Qian"
val Name(first, last) = author // call Name.unapply(author)
// first == "Baihu", last = "Qian"
```
Note `Name` is an object.

## Package and Import
Packages provide namespace management, and Import provides a shorter alias.

### Package
There are two major styles to add items to a package: `package` statement at the top of a file, like Java, or curly brace packaging style like C++ and C#:
```scala
package example {
	class Example1 {
		...
	}
}
```

A package can be defined in multiple files, and a file can contribute to multiple packages by using the curly brace style.

Package provides more consistent scope. Everything in parent scope is in scope so that package names can be *relative*, unlike Java.

Package names can be chained together using `.`. Everything in chained scope is no longer visible. For example, in `com.baihu.test` everything in `com` and `com.baihu` packages are not visible.

If all the code in one file belongs to the same package, you can use top-of-file notation:

```scala
package com.baihu.test
package people

class Person {
	...
}
```

Then `Person` class is in `com.baihu.test.people` package but everything in `com.baihu.test` is visible.

#### Package objects
Package cannot include definitions of functions and variables. Package objects address this limitation.

Put the package object into a file called `package.scala` inside the package. The package object definition is like this (assuming package is called `com.baihuqian.test.model`):

```scala
// filename: src/com/baihuqian/test/model/package.scala
package com.baihuqian.test // model is not here

package object model {
    
}
```
Those defined in the package object can be directly accessed from within other classes, traits, objects in the package.

#### Package Visibility
In Java, a class member without access quantifers are visible in the package containing the class. In Scala, you can use `private[packageName]` to expose access to a certain package.

### Import
Scala import is like Java, but more flexible. Imports can be anywhere, no just at the beginning of a file. The scope of import extends in the enclosing block. Scala uses `_` (instead of `*`) for wildcard.

Each Scala program implicitly imports `java.lang._` and `scala._` package, and `Predef` object. `Predef` object provides many implicit type conversions as well as methods like `println`, `readLine`, `assert`, and `require`.

It is possible to import multiple classes in one line: `import java.awt.{Color, Font}`.It is known as *import selector clause*.

Scala let you import members from a class as well. It is similar to Java *static import* (import static methods directly), but it is also able to import values.

#### Renaming and Hiding
Renaming: `import java.util.{HashMap => JavaHashMap}`. It is useful when importing several classes with the same name. Not only can you rename classes on import, but you can also rename class members as well.

Hiding: `import java.util.{HashMap => _, _}`. The selector `HashMap => _` hides `HashMap` instead of renaming it. So the above line imports everything except `HashMap`. Note the wildcard `_` must be at the end of the import clause.

## Block statement
The curly bracket `{}` is optional. It is required when multiple statements are inside if/else branch or loop statement that spans multiple lines, or on the same line separated by semicolons.

If needed, Scala programmers favor the Kernighan & Ritchie brace style:

```scala
if (n > 0) {
	r = r * n
	n -= 1
}
```

The result of a block statement is the value of the last expression. It is useful when initialization of a `val` takes multiple steps.

## Input and output
### Text
#### Output
`print()` and `println()`: the latter adds a newline to the end.

`printf()` supports C-style format string.

#### Input
`readLine()` read a line of input. It can specify a prompt string.

`readInt(), readDouble()` etc can read a proper type value, but no prompt string can be supplied.

### File
#### Input
* Open a file: use `scala.io.Source` object: `val source = Source.fromFile(filename, encoding)`. Filename can be a string or a `java.io.File`. Encoding can be omitted.
* Read from other sources: use `Source.fromURL(URL, encoding)`, `Source.fromString(s)`, and `Source.stdin` to read from URL, String, and standard input.
* Read lines: `val lineIterator = source.getLines`. The result is an iterator. It can be used to process line-by-line in a for loop, or convert to array/buffer using `toArray`/`toBuffer`.
* Read whole file: `source.mkString`.
* Read character: use `source` object directly. If you want to peek without moving the header, call `val iter = source.buffered` to get a buffered object.
* Split by token: `val tokens = source.mkString.split("\\s+")` splits by whitespace.

Binary files: Scala has no specific way to read binary files. Use Java library instead.

#### Output
To write a text filem use `java.io.PrintWriter`.

#### Directories
Fall back to Java libraries.

#### Serialization
`@SerialVersionUID(0L) class Person extends Serializable`. Then to send an object, use instance of `ObjectOutputStream` and call `writeObject(obj)` method. To receive an object, use instance of `ObjectInputStream` and call `in.readObject().asInstanceOf[Class]` to convert to the right type.

Scala collections are serializable.


## Exception
Scala exception works the same way as in Java. But Scala has no **"checked"** exceptions, i.e. you never have to declare a function of method might throw and exception. However, if you want, you can still do so, and it is required if the method will be called from Java code:

```scala
@throws(classOf[NumberFormatException])
def toInt(s: String) = s.toInt
```

A `throw` expression has the special type `Nothing`. If one branch in `if/else` is `Nothing`, the type of the `if/else` is the type of the other branch.

The syntax of catch exceptions is modeled after the pattern matching syntax.

`finally` clause is the same as in Java.

# Values and Variables

Values are defined using `val` keyword.

```scala
val const = 2
```

Values are constant, it cannot be changed once it's defined:

Variables are defined using `var` keyword:

```scala
var answer = 1
```
	
defines a variable named answer.

In Scala, `val` is encouraged if you don't need to change the content. It is generally better for functional style to use immutables.

It is an **error** to declare a value or variable without initializing it, i.e. assigning a content to it.

## Type
Type of value or variable is inferred from the expression with which you initialize it, so no explict type declaration is needed. However, you can declare it:

```scala
val greetings: String = "Hello"
val st: Short = 0
val greetingObject: Object = greetings
```
It is useful to explicitly override the default type of literals, or cast the type at assignment.
	
Note: the type of a variable or function is always written **after** the name of the variable or function.

You can declare multiple values or variables together:

```scala
val xmax, ymax = 100 // xmax and ymax are set to 100
val (x, y) = (1, 2)
```

Assignment has a value of () `Unit`. Thus you **cannot chain** them together.

Use `_` for variables that you don't need it.

`classOf[T]` method, defined in `Predef` object, retrieves the runtime representation of a class type. `getClass` method, defined in `Any` class and available to all Scala classes, retrieves the runtime representation of an object.

### Type Cast
Use `asInstanceOf[TypeName]` method to cast to the desired type. It is defined in the Scala `Any` class and is therefore available on all objects.

## Lazy values
When a `val` is declared as `lazy`, its **initialization** is deferred until it is accessed for the first time. It is useful to delay costly initialization process. But lazy value is inefficient since it is checked for initialization before every use.

lazy is halfway between `val` and `def`:

* `val s = "hello" // Evaluated as soon as its defined`
* `lazy val s = "hello" // Evaluated at the first time it is used`
* `def s = "hello" // Evaluated every time it is used`

## Data Types
### Numeric Types
Seven numeric types: `Byte`, `Char`, `Short`, `Int`, `Long`, `Float`, `Double`. Unlike Java, they are objects. They have the same data ranges as their Java primitive equivalents.

|Data type| Range |
|:-|:-|
|Char|16-bit unsigned Unicode character|
|Byte|8-bit signed value|
|Short|16-bit signed value|
|Int|32-bit signed value|
|Long|64-bit signed value|
|Float|32-bit IEEE 754 single precision float|
|Doublle|64-bit IEEE 754 single precision float|

Each type has `MinValue` and `MaxValue` constants to show the range. `Double.PositiveInfinity` and `Double.NegativeInfinity` offers nice representative of infinities.

`RichInt`, `RichDouble`, `RichChar`, so on, offers more convenience methods. 

#### BigInt and BigDecimal
`BigInt` and `BigDecimal` offers more digits to store the number. Unlike Java (and thanks to Scala), you can use natural operators, such as + and -. You can convert to basic numeric types via `to<Type>` method. It also have check for conversion safety (see below "Conversion").

#### Conversion
`to<Type>` converts String to numeric types, and between numeric types, e.g.

```scala
val i: Int = "100".toInt
val d: Double = i.toDouble
```

`BigInt` and `BigDecimal` can be created directly from String:
```scala
val b = BigInt("1")
```

These methods will throw the usual Java `NumberFormatException`. When converting between numeric types, `isValid<Type>`, offered by `Rich<Type>`, tests whether conversion would be successful.

#### Random Number
Use `scala.util.Random` class.

* `setSeed`: set the random seed. You can also pass the seed when creating the `Random` object.
* `next<Type>`: generate a random number. For `nextInt`, it takes an optional maximum value, and return a number between 0 (inclusive) and the value you specify (exclusive).
* `nextPrintableChar`: generate a random character.

### Basic type
A `Boolean` type.

`Unit`: placeholder for "no useful value" in Scala, written as `()`. It is an analog of `void` in Java/C++.

#### Basic type extension
* Spire project: more numerial types like `Rational, Complex, Real`.
* ScalaLab: Matlab-like scientific computing in Scala.
* nscala-time: a wrapper around Joda-Time that offers more Scala style coding.

### String
Scala `String` is Java String class. String literals are enclosed with double quotes (`"`). Multi-line string literals are enclosed in triple double quotes:

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

#### String substitution
Prepending `s` to any string literal allows the usage of variables directly in the string.

`s"Hello, $name"` interpolates the string with variable `name`. String interpolators can also take arbitrary expressions, and they should be enclosed in `${}`.

If the string contains escaped double quotes, use triple quotes instead: `s """This is a quote: \"$quote\""""`.

#### `printf` style formatting

Prepending `f` to any string literal allows the creation of simple formatted strings, similar to printf in other languages. When using the `f` interpolator, all variable references should be followed by a `printf`-style format string, like `%d`. For example, `f"$name%s is $height%2.2f meters tall"`.

The `f` interpolator is typesafe. If you try to pass a format string that only works for integers but pass a double, the compiler will issue an error.

#### The `raw` Interpolator

The `raw` interpolator is similar to the `s` interpolator except that it performs no escaping of literals within the string. 

#### Useful APIs
* `capitalize`: convert a string to upper case.
* `split`: takes a String, a Char, or regular expressions and splits the string.
* `replaceAll`: It replaces all occurrences (can be specified with regex) with another string. `replaceFirst` does the same for first occurrence only.

#### Regular Expressions
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

##### Extract matched patterns
Use groups to extract subexpressions from matches. Put parentheses around the subexpression to form a group. For example,

	val pattern = "([0-9+]) ([a-z]+)".r

Then `val pattern(num, item) = str` assign `num` to match of `[0-9+]` and `item` to match of `[a-z]+`. You can think of this as `unapply` method.
	





 
# Flow Control 
## Condition
`if/else` returns a value, which is the value of the expression that is true after evaluation. For example,

```scala
val s = if (x > 0) 1 else -1
```
	
is the same as
	
```scala
if (x > 0) s = 1 else s = -1
```
	
In the first form, it can initialize a `val` but in the second form `s` needs to be a `var`.

The expression is similar to `?:` in C++ and Java, but if/else in Scala can have statements inside.

An `if` statement without the `else` clause can yield a `()` if evaluation fails. So `if (x > 0) 1` is equivalent to `if (x > 0) 1 else ()`.


There are no `switch` statement in Scala. Use pattern matching instead. `@switch` can be applied to improve performance from decision tree to branch table at compile time, with a few constraints.

## Loop
Scala has the same while and do-while loops as Java and C++:

```scala
while (n > 0) {
	r = r * n
	n -= 1
}

do {
	r = r * n
	n -= 1
} while (n > 0)
```
	
for loop is more like Python:

```scala
for (i <- 1 to n)
	r = r * i
```
Ranges created with the `<-` symbol in `for` loops are refereed to as *generators*. There are no `break` and `continue` keywords in Scala.

Note `i to n` returns a `Range` from 1 to n inclusive, and `<-` makes `i` traverse all values of the expression at its right side.

In Scala, it is possible to apply a function to all values in a sequence at once, reducing the use of loops.

### Advanced `for` Loop Usage
#### Multiple counters
Multiple *generators* of the form `variable <- expression` separated by semicolon:

```scala
for (i <- 1 to 3; j <- 1 to 3) print(i + j)
```

It is preferred to write it in this form:

```scala
for {
    i <- 1 to 3
    j <- 1 to 3
} print(i + j)
```

#### Guard
Each generator can have a *guard*, a Boolean condition by `if`:

```scala
for (i <- 1 to 3; j <- 1 to 3 if i != j) print(i + j)
```

No semicolon before the `if`. 

You can define variables to use in `for` without the `var` keyword:

```scala
for (i <- 1 to 3; from = 4 - i; j <- from to 3) print (i + j)
```
	
The content of a `for` loop can be expressed in braces, replacing semicolons with newlines:

```scala
for { i <- 1 to 3
	from = 4 - i
	j <- from to 3
}
```

`for` loop with counters (like Go): Scala collections offer a `zipWithIndex` method that create a new collection with `(elem, counter)` as elements.

### `for` Comprehensions
When the body of the `for` loop starts with `yield` keyword, then the `for` loop constructs a collection of values. For example,

```scala
for {
	i <- 1 to 3
	square = i * i
} yield square
```

constructs a collection. Note variables inside `for` loop don't need a `var` or `val` quantifier. If the statement after `yield` requires multiple lines, enclose them in `{}`. In most cases, the `for/yield` loop returns the same type of collections as input.

For comprehension is essentially the same as `map` method available on `Iterable`.

### Patterns in `for` expressions
`for ((k, v) <- map) println(k + v)` traverses the maps and output keys and values bound to `k` and `v`. Match failures are silently ignored.

Guard can be applied, but it goes after the `<-` symbol.

### How Scala Compiler works with `for` loops
1. A simple `for` loop that iterates over a collection is translated to a `foreach` method call on the collection.
2. A `for` loop with a `yield` expression is translated to a `map` call on the collection.
3. The *guard* is translated to one or more `withFilter` calls on the collection prior to `foreach` or `map` call.

Thus it is more straightforward to avoid explicit for loop but use `map`, `foreach`, and `filter` directly.

## `break` and `continue`
There are no `break` and `continue` keywords, but similar functionality can be implemented with `scala.util.control.Breaks`. It offers two methods, `break` and `breakable`. `break` will throw a `BreakControl` exception and `breakable` will catch it.

### Break
```scala
breakable {
    for (x <- xs) {
        if (cond)
            break
    }
}
```

### Continue
```scala
for (x <- xs) {
    breakable {
        if (cond)
            break
    }
}
```

### Labeled Break
`Breaks` object can be instantiated, and `breakable` will only catch exception thown by `break` in the same object.

### Enumeration
Unlike Java, Scala does not have enumeration. Standard library provides an `Enumeration` helper class.

Define enumeration as follow:

```scala
object TrafficLightColor extends Enumeration {
	val Red, Yellow, Green = Value
}
	
```

Each gets an ID and name: ID defaults to one more than the previous one, starting from zero; name is the variable name.

So the enumeration can be accessed by `TrafficLightColor.Red`, etc.

You can look up an enumeration value by its ID or name:

```scala
TrafficLightColor(0)
TrafficLightColor.withName("Red")
```

# Application
## Launch an Application
### The `App` trait
An object that extends the `App` trait becomes the entry point of a Scala application, and the body of the object will be run. Any command-line arguments will be available through the `args` object of type `Array[String]`, which is inherited from the `App` trait.

### Define `main` Method
An object that contains a `main(args: Array[String])` method. It is similar to Java.

## Execution of shell commands
Use `scala.sys.process` package.

Put commands in quotes and append `!` or `!!`: `"ls -al .." !`

* `!`: return value is the exit code;
* `!!`: return value is the actual output of the command.

Prefix # to the following operators:

* `|` is supported, use `#|`. All commands are in quotes.
* Redirect output (`>`) using `#>`.
* Append to a file (`>>`) using `#>>`.
* Redirect input (`<`) using `#<`.
* Execute q if p was successful: `p #&& q`.
* Execute q if p was unsuccessful: `p #|| q`.


  [1]: ##Function
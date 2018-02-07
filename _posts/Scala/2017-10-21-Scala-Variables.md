---
title: Learning Scala - Variables and Types
tags: Scala
---

Unlike Java, Scala provides both immutable "values" and mutable "variables", to reflect its functional programming features.

Values are defined using `val` keyword.

```scala
val const = 2
```

Variables are defined using `var` keyword:

```scala
var answer = 1
```

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

